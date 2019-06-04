---
layout: post
title: General tips for debugging/profiling/benchmarking
date: 17-07-14 16:40
tags: [debugging, profiling, benchmarking]
category: Notes
author: Rohan Garg
summary: General tips for debugging/profiling/benchmarking
---

In general, there's no fixed rule of thumb that one could use for all
situations, especially with debugging. I think one just gets good at
recognizing common patterns over time. That said, reading postmortems
and debugging stories is quite useful.

Kapil and I were discussing this once when he mentioned that perhaps
the most general rule of thumb is to continuously challenge each and
every assumption, and I agree. I'd also say that gather as much evidence
(or data) as possible and think of the implications. In other words,
hypothesize about a possible path that your program might have taken,
given the evidence, and compare that with the actual implementation.

Here are some recent interesting benchmarking/profiling/debugging stories
I have read.

 - [24-core CPU and I can’t move my mouse](https://randomascii.wordpress.com/2017/07/09/24-core-cpu-and-i-cant-move-my-mouse/)
 - [Fewer mallocs in curl](https://daniel.haxx.se/blog/2017/04/22/fewer-mallocs-in-curl/)
 - [How is GNU `yes` so fast?](https://www.reddit.com/r/unix/comments/6gxduc/how_is_gnu_yes_so_fast/)
 - [Treadmill: Attributing the Source of Tail Latency ...](http://yunqi.cat/pdf/zhang2016treadmill.pdf)

Most the stuff that Julia writes on her blog is good, especially for
when one is starting out. Her approach and exposition very clear;
here are some good articles.

 - [Linux tracing systems & how they fit together](https://jvns.ca/blog/2017/07/05/linux-tracing-systems/)
 - [How I got better at debugging](https://jvns.ca/blog/2015/11/22/how-i-got-better-at-debugging/)
 - [Linux debugging tools](https://jvns.ca/zines/#linux-debugging-tools)
 - [On strace](https://jvns.ca/strace-zine-v2.pdf)
 - [How to ask good questions](https://jvns.ca/blog/good-questions/)

Anything from Dan Luu or Brendan Gregg is usually quite good.

 - [Why don't schools teach debugging?](http://danluu.com/teach-debugging/)
 - [perf Examples](http://www.brendangregg.com/perf.html)

Here's one great piece of advice on benchmarking from Gernot Heiser
(of the seL4 fame):

 - [Systems Benchmarking Crimes](https://www.cse.unsw.edu.au/~gernot/benchmarking-crimes.html)
 - https://www.cse.unsw.edu.au/~cs9242/16/lectures/05-perfx4.pdf

GDB tricks (from Gene):

 - https://course.ccs.neu.edu/cs5650/#debugging
