---
layout: post
title: Verifying DMTCP's Jalloc Implemenation
date: 17-07-14 16:34
tags: [dmtcp, formal, tla+]
category: Notes
author: Rohan Garg
summary: Verifying DMTCP's Jalloc Implemenation
---


I work on the [DMTCP](http://dmtcp.sf.net) checkpointing project. My
day-to-day work primarily deals with low-level implementation issues in
C/C++/ASM and other research questions.

This is regarding a bug that we recently discovered (and haven't yet
fixed) in our implementation.

DMTCP uses a custom memory allocation library (called Jalloc). The
memory allocation algorithm is thread-safe and async-signal safe and
is implemented as a lock-free stack, where a _root_ pointer represents
the top of the stack.  Being thread-safe *and* async-signal safe is
a key requirement for correctness and functionality; performance is a
secondary concern.  The root pointer is changed atomically (using the
atomic compare-and-swap CPU instruction). The main invariant for the
algorithm is that the root must point to either NULL (in case the stack
is empty) or a valid memory address in all possible cases.

The bug we had observed was that, for this one multithreaded program with
a specific pre-condition, the root would become invalid (i.e., point to
an invalid memory address). This would eventually lead to the program
crashing. Note that this implementation and the multithreaded test
program have been a part of our distribution probably since inception,
and moreover there are many users out there using this. So it was quite
a surprise when we discovered this.

After struggling to explain the observations for almost two weeks, we
finally traced it down to an ABA problem. It took us a while because
of the slightly unconventional structure of our data structures that
use C-based structs and unions. So, while the definitions and examples
of the ABA problem are easily available on the Internet, we couldn't
directly relate any one of them to our bug/observations.

So we figured out the bug and a potential fix (using a double-width
CAS). But that left me uneasy. The bug manifests itself because of a
very specific interleaving of threads at the assembly level --- three
interleavings in the middle of three specific assembly instructions.

I started looking into TLA+ this Spring as a means of modeling the
semantics of concurrent, multithreaded programs, as a side project.
My first aim was to model the Jalloc implementation and have TLA+
reproduce the bug.

After some work, I was able to reproduce the same bug with a +Cal/TLA+
model. Running the model with just two threads and the temporal invariant
that the root should never be invalid reproduces the bug trace.

Next, I set about trying to model and verify the fix in another TLA+
model. But this turned out to be more interesting than I had anticipated.

The most surprising and pleasant thing for me was that my initial
understanding about the bug fix turned out to be flawed or incomplete;
TLA+ managed to point out the flaw! The key insight was that looking at
the C code isn't enough, and one must look at the compiler-generated
assembly. If the structure layout and the generated assignment
instructions are not in a specific format, there are cases where the
invariant would break because of certain interleavings. Of course,
it would have been nearly impossible to test for this with an actual
implementation; what appears to work prima facie might not be correct
for all cases.

Finally, it works after the fix, or at least, the model checker doesn't
report any violations of the main invariant with 2 threads. (Note
that 2 threads were enough to demonstrate the bug with the incorrect
implementation.)

With two threads, it takes 2-3 seconds to verify the fixed version.
With three threads, it takes up to 1.5 minutes. (I haven't tried
with four threads.) The timings also depend on the machine. These
numbers are from my laptop (2.3 GHz Core-i7/8 GB RAM).  It's actually
much faster on my lab workstation (3.1 GHz Xeon E3-1220 v3/16 GB
RAM).

While the number of threads is a parameter to the model, and I just
have to re-run with a different number without changing anything
in the code, it does make me uneasy. More specifically, it's unclear
to me that after testing with N threads it'll continue to work with
N+1 threads.

Since I know the C/ASM code well enough in this particular example,
I'm reasonably confident that what I have is correct.  But I do
have concerns about applying this to other domains where I have no
prior knowledge.

That said, the model checker systematically tests for all the possible
thread interleavings, which is better than nothing for me. This has
given me more confidence and insights about the proposed fix.

To summarize, I'm quite satisfied with +Cal/TLA+ and would be
happy to incorporate it in other areas of my work.

(We are planning to commit the TLA+ code to our public code repository.
 For example, see [here](https://github.com/dmtcp/dmtcp/pull/602/files))
