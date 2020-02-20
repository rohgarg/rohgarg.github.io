---
layout: post
title: TLS in Linux/x86
date: 2020-02-20 10:25
category: Notes
author: Rohan Garg
tags: [linux, tls, fsgs]
summary: Various notes on TLS in Linux
---

While implementing thread preemption in user-space, it's important to
note that `getcontext()` and `setcontext()` do not save and restore
the $fs register, which is used to implement TLS in Linux. This needs
to be done explicitly. (Thread context switches in the kernel involve
saving and restoring of the FS register, among others.)

Currently, the only way to do this on mainline kernel is through a
syscall (`arch_prctl()`), which can be pretty expensive -- changing
the $fs base is a privileged instruction. And so, language VM's like
JVM cannot easily implement light-weight, user-space threads. Newer
Intel CPUs (post Ivy Bridge) introduced a new unprivileged instruction
(`FSGSBASE`) to modify the register value, but the kernel patch to
make it available to the users hasn't been accepted yet.

The $fs register is also used to refer to the thread-local variables
declared in the source code (`__thread`).

Even without any thread-local vars in a program (`__thread`), by
default, there will be at least one: `errno`. `errno` is used by
kernel/libc to indicate errors when a system call fails. Since each
pthread can execute system calls independently, a thread-local `errno`
is a good way to return errors to each thread (without causing
confusion!). So, at the bare minimum, there will be a TLS region of
size 4 bytes.
