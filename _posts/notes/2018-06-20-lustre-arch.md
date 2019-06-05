---
layout: post
title: Notes on the Lustre architecture
date: 2018-06-20 13:20
category: Notes
author: Rohan Garg
tags: [lustre]
summary: Dev notes on the architecture of Lustre and its software components
---

#### client vfs (llite)

The client VFS interface, also called llite, is the bridge between
the Linux kernel and the underlying Lustre infrastructure represented
by the LOV, MDC, and LDLM subsystems. This includes mounting the
client filesystem, handling name lookups, starting file I/O, and
handling file permissions.

    lustre/llite/

#### client vm

Client code interacts with VM/MM subsystems of the host OS kernel
to cache data (in the form of pages), and to react to various
memory-related events, like memory pressure.

#### client i/o

Client I/O is a group of interfaces used by various layers of a
Lustre client to manage file data (as opposed to metadata). Main
functions of these interfaces are:

- Cache data, respecting limitations imposed both by hosting MM/VM,
and by cluster-wide caching policies, and

- Form a stream of efficient I/O RPCs, respecting both ordering/timing
constraints imposed by the hosting VFS (e.g., POSIX guarantees,
O_SYNC, etc.), and cluster-wide IO scheduling policies.

Client I/O subsystem interacts with VFS, VM/MM, DLM, and PTLRPC.

    lustre/obdclass/cl_io.c

#### client metadata (mdc)

The Metadata Client (MDC) is the client-side interface for all
operations related to the Meta Data Server MDS. In current
configurations there is a single MDC on the client for each filesystem
mounted on the client. The MDC is responsible for enqueueing metadata
locks (via LDLM), and packing and unpacking messages on the wire.

    lustre/mdc/

#### lov

The LOV device presents a single virtual device interface to upper
layers (llite, liblustre, MDS). The LOV code is responsible for
splitting of requests to the correct OSTs based on striping information
(lsm), and the merging of the replies to a single result to pass
back to the higher layer.

    lustre/lov

#### libcfs

Libcfs provides an API comprising fundamental primitives and
subsystems - e.g. process management and debugging support which
is used throughout LNET, Lustre, and associated utilities.

#### ptlrpc

All communication between Lustre processes are handled by RPCs, in
which a request is sent to an advertised service, and the service
processes the request and returns a reply. Note that a service may
be offered by any Lustre process -- e.g., the OST service on an OSS
processes I/O requests and the AST service on a client processes
notifications of lock conflicts.

#### llog

General logging mechanism

#### ldlm

Lustre distributed lock manager

#### fid

Unique object identifier

#### ost

Thin layer that translates RPCs into local obdfilter calls.

#### ldiskfs osd

diskfs-OSD is an implementation of `dt_{device,object}` interfaces on top of
(modified) ldiskfs file-system.

It uses standard ldiskfs/ext3 code to do file I/O.
