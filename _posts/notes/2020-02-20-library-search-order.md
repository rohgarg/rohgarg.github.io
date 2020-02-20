---
layout: post
title: Linux ld library search order
date: 2020-02-20 10:21
category: Notes
author: Rohan Garg
tags: [linux, ld, linking, library]
summary: Library search order for static linking
---

While resolving dependencies at runtime, `DT_RUNPATH` is not
transitive. If `DT_RUNPATH` is set for the base executable, runtime
linker/loader (RTLD, or `ld.so` on Linux) won't be able to resolve all
the indirect dependencies, unless each of the direct dependencies also
specifies `DT_RUNPATH`. And so, one has to set `DT_RUNPATH` for all
the dependencies -- direct and indirect. In contrast, `DT_RPATH`
applies transitively.

Newer linkers probably only do `DT_RUNPATH`. `DT_RPATH` is an old
thing (it's deprecated?? I think).  The library search order goes
something like:

 1. `DT_RPATH` (if `DT_RUNPATH` is not there)
 2. `LD_LIBRARY_PATH`
 3. `DT_RUNPATH`
 4. ldconfig
 5. standard paths (/usr/lib, /lib, etc.)

To check what's there in the binary/so run: `readelf -a <binary> | grep PATH`.
Here's a sample output:

```
$ readelf -a ./a.out  | grep PATH
 0x000000000000001d (RUNPATH)     Library runpath: [/home/user/proj/dmtcp/lib/]
```

It seems like even if one uses `-Wl,-rpath=/path/to/dir/with/libs`
while linking, it only sets `DT_RUNPATH`. So, the command line option
is kind of misleading (perhaps, it should explicitly say `-runpath`).

Force setting of `DT_RPATH` is not recommended. It's probably a relic
from the old days. It's a big hammer for a small thing. For example,
usage of this option would preclude one from using `LD_LIBRARY_PATH`
to overrided some libraries, because `DT_RPATH` has a higher
precedence in the search order.

However, if one does want to force set `DT_RPATH`, one could use
`-Wl,--disable-new-dtags` to force ld to use the older methods.
