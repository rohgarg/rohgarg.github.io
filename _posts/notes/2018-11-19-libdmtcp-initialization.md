---
layout: post
title: Libdmtcp initialization
date: 2018-11-19 10:21
category: Notes
author: Rohan Garg
tags: [dmtcp, libdmtcp]
summary: Regarding the initialization sequence in libdmtcp
---

There appears to be a fundamental flaw in the design of the initialization
within libdmtcp.

The current design is this:

```
dmtcp_initialize() // Single point of entry for initialization
  if (initialized)
    return;
  ...
  Init_Wrappers()
  Init_Ckpt_Thread()
  

DmtcpWorker()    // libdmtcp Constructor
  dmtcp_initialize()

dmtcp*wrapper()  // Any wrapper function
  if (!initialized)
    dmtcp_initialize()
  ...
```

In the previous design, all the initialization work used to
happen in the constructor. However, that design failed because some
base library (e.g., libmpi.so), which gets initialized ahead of
libdmtcp.so, could call a wrapper function in its constructor. We
had multiple hacks to make this work.

As a result, we came up with a new design (the current design), where
we removed this dependency on the `DmtcpWorker` constructor and created
a single point-of-entry for initialization: `dmtcp_initialize()`. This
way even if a wrapper function is called by some library before control
gets to `DmtcpWorker`, we do the right thing.

This design has been there for a while and seems to work in most
cases. But I realized today that there's a fundamental flaw.

The core issue is that before the control reaches the `DmtcpWorker`
constructor, we are not sure that the malloc library (which is a
base library, along with libc, libpthread, etc.) has been initialized
or not.

So, this design breaks down when a malloc library (e.g., jemalloc)
calls a wrapper function during its initialization, which then calls
some libc function that wants to call malloc again. For example,
even creating the ckpt thread requires a call to pthread_create(),
which wants to call malloc.

This brings us to one possible solution. I think the key principle
we need to follow is this:

> Until the control gets to the `DmtcpWorker` constructor, we cannot
> directly or indirectly (through a libc call) call a function that
> can call malloc.

The idea is to split the initialization of libdmtcp into two
phases: (a) one which is safe to do before the constructor is called, and
(b) one which is safe to do after the constructor has been called.

For (a), we need to ensure that any of the wrapper functions does
not directly or indirectly call malloc. (Note: we can still
call our internal JALLOC allocation calls during any of the phases.)
