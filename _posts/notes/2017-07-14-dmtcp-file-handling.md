---
layout: post
title: DMTCP File handling background
date: 17-07-14 16:42
tags: [dmtcp, checkpointing]
category: Notes
author: Rohan Garg
summary: DMTCP File handling background
---

Here's some bit of background on DMTCP file handling:

One can either invoke DMTCP with the `--checkpoint-open-files` option
or have it take care of checkpointing files with predefined rules for
different situations. If invoked as: `dmtcp_launch --checkpoint-open-files a.out`,
then DMTCP saves the contents of all open files, and restores them
at restart time.

The default behavior of DMTCP in checkpointing open files (i.e., without
the `--checkpoint-open-files` option) is:

 1) if the file is open as read-only, do not assume write permission,
    and do not save its copy.

 2) if the file is open with write permission, and the offset is
    at the end of the file, assume that the file is a log file. Do
    not save, and on restart, set the file descriptor back to the
    offset saved at checkpoint time, which effectively truncates
    the file.

 3) if the file is open with write permission, and the offset is
    not at the end of the file, assume that the file is a database
    file. Save the file contents and restore at restart time.


Note: If DMTCP is invoked without the `--checkpoint-open-files` flag,
the files that the application was using at checkpoint time are
expected to be at their original location (in cases (1) and (2)
above). On restart, the file descriptors are restored with the
offsets saved at checkpoint time as described in the cases (1),
(2), and (3) above.

Here are some more details about the `--checkpoint-open-files` flag:

When using this flag, at checkpoint time, after the user threads
are frozen, DMTCP creates a local directory and saves copies of all
files with open file-descriptors. Since all the user threads are
frozen, we are sure no one is modifying the process memory and the
files.

At the time of restart, DMTCP checks if a file that it had saved
at checkpoint time is present at its original location. If the file
is present, DMTCP checks if the file contents are the same as what
it had saved. It asserts out if the file contents differ. We take
this approach because we don't want to accidentally overwrite user
files.

There are three other file related options:

 - pathvirt

    This option virtualizes access to file system paths for programs
    run under DMTCP. This allows path access to be transparently
    redirected elsewhere on the file system on DMTCP restart. Here's
    a motivating example: when checkpointing as user-A and restarting
    as user-B, files being used in user-A's home directory will no
    longer be accessible, and a user might want to redirect path
    accesses to user-B's files.

 - ckptfiles

    The problem with the default `--checkpoint-open-files` option
    is that it saves all open files. This option allows a user to
    specify exactly the files they want to save. A motivating use-case
    is that a user might not want to save temporary logs files created
    in a particular directory to improve checkpoint/restart time.

 - allow-file-overwrite

    If used with `--checkpoint-open-files`, allows a saved file to
    overwrite its existing copy at original location. Recall that
    the default behavior with `--checkpoint-open-files` does not allow
    file overwrites; if the file contents of the saved copy of a
    file differ from its existing copy, DMTCP asserts out with a
    warning.

The takeaway is that it's difficult to have a one-size-fits-all solution
for files. The good thing is that there are various runtime options,
callbacks and plugins, that can help a user customize the behavior.
