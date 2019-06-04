---
layout: post
title: Systems readings
date: 2013-03-01 11:26
tags: [readings, systems]
category: Readings
author: Rohan Garg
summary: List of readings
---

#### Kernels
  -  D. M. Ritchie and K. Thompson. **The unix time-sharing system.** Commun. ACM,
  17(7):365-375, 1974.
  - D. R. Engler, M. F. Kaashoek, and J. O`Toole, Jr. **Exokernel: an operating
  system architecture for application-level resource management.** In SOSP 1995.
  - Rob Pike, Dave Presotto, Sean Dorward, Bob Flandera, Ken Thompson, Howard
  Trickey, Phil Winterbottom. **Plan 9 from Bell Labs.** Computing Systems,
  8(3):221-2254, 1995.
  - E. Bugnion, S. Devine, K. Govil, and M. Rosenblum. **Disco: running commodity
  operating systems on scalable multiprocessors.** ACM Trans. Comput. Syst.,
  15(4):412-447, 1997.



#### Distributed Systems

  - Lamport, Leslie. **The part-time parliament.** ACM Trans. Comput. Syst. 16,
  no. 2 (1998): 133-169. [Link](http://dx.doi.org/10.1145/279227.279229)
  - Castro, Miguel, and Barbara Liskov. **Practical Byzantine fault
  tolerance.** In OSDI 1999. [Link](http://www.pmg.csail.mit.edu/papers/osdi99.pdf)
  - Lamport, Leslie, Robert Shostak, and Marshall Pease. **The Byzantine Generals
  Problem.** ACM Trans. Program. Lang. Syst. 4, no. 3 (1982): 382-401.
  [Link](http://dx.doi.org/10.1145/2402.322398)

#### Memory Management

  - R. C. Daley and J. B. Dennis. **Virtual memory, processes, and sharing
  in multics.**  Commun. ACM, 11(5):306-312, 1968.
  - R. W. Carr and J. L. Hennessy. **Wsclock-a simple and effective algorithm for
  virtual memory management.** SIGOPS Oper. Syst. Rev., 15(5):87-95, 1981.
  - R. Rashid, A. Tevanian, M. Young, D. Golub, R. Baron, D. Black, W. Bolosky,
  and J. Chew. **Machine-independent virtual memory management for paged
  uniprocessor and multiprocessor architectures.** SIGARCH Comput. Archit. News,
  15(5):31-39, 1987.
  - Jacob, B., and T. Mudge. **Software-managed address translation.** In HPCA 1997.
  [Link](http://dx.doi.org/10.1109/HPCA.1997.569652)
  - Witchel, Emmett, Josh Cates, and Krste Asanovic. **Mondrian memory protection.**
  ASPLOS 2002. [Link](http://dx.doi.org/10.1145/605397.605429)
  - J. Navarro, S. Iyer, P. Druschel, and A. Cox. **Practical, transparent
  operating system support for superpages.** In OSDI 2002.

#### Synchronization

  - S. Savage, M. Burrows, G. Nelson, P. Sobalvarro, and T. Anderson. **Eraser:
  a dynamic data race detector for multithreaded programs.** ACM
  Trans. Comput. Syst., 15(4):391-411, 1997.
  - P. E. McKenney, J. Appavoo, A. Kleen, O. Krieger, R. Russell, D. Sarma,
  and M. Soni. **Read-copy update.** In Ottawa Linux Symposium, July 2001.

#### Scheduling

  - B. N. Bershad, T. E. Anderson, E. D. Lazowska, and H. M. Levy. **Lightweight
  remote procedure call.** ACM Trans. Comput. Syst., 8(1):37-55, 1990.
  - Anderson, Thomas E., Brian N. Bershad, Edward D. Lazowska,
  and Henry M. Levy. **Scheduler activations: effective kernel
  support for the user-level management of parallelism.** In SOSP 1991.
  [Link](http://dx.doi.org/10.1145/121132.121151)
  - Adya, Atul, Jon Howell, Marvin Theimer, William J. Bolosky,
  and John R. Douceur. **Cooperative Task Management
  Without Manual Stack Management.** In USENIX ATC, 2002.
  [Link](http://www.usenix.org/publications/library/proceedings/usenix02/adyahowell.html)
  - Soares, Livio, and Michael Stumm. **FlexSC: flexible system
  call scheduling with exception-less system calls.** In OSDI 2010.
  [Link](http://www.usenix.org/events/osdi10/tech/full_papers/Soares.pdf)

#### Security

  - Lampson, Butler W. **Protection.** SIGOPS Oper. Syst. Rev. 8, no. 1
  (1974): 18-24. [Link](http://dx.doi.org/10.1145/775265.775268)
  - Dunlap, George W., Samuel T. King, Sukru Cinar, Murtaza A. Basrai, and
  Peter M. Chen. **ReVirt: enabling intrusion analysis through virtual-machine
  logging and replay.** In OSDI 2002. [Link](http://dx.doi.org/10.1145/844128.844148)
  - Zeldovich, Nickolai, Silas Boyd-Wickizer, Eddie Kohler, and David Mazières.
  **Making information flow explicit in HiStar.** Commun. ACM
  54, no. 11 (2011): 93-101. [Link](http://dx.doi.org/10.1145/2018396.2018419)

#### File systems

  - McKusick, Marshall K, William N Joy, Samuel J Leffler, and Robert S
  Fabry. **A fast file system for UNIX.** ACM Transactions on Computer
  Systems (TOCS) 2 (August 1984): 181–197. [Link](http://dx.doi.org/10.1145/989.990)
  - Sandberg, R., D. Golgberg, S. Kleiman, D. Walsh, and B. Lyon. **Design
  and implementation of the Sun network filesystem.** In Proc. USENIX Summer
  Technical Conference 1985.
  - Rosenblum, Mendel, and John K. Ousterhout. **The design
  and implementation of a log-structured file system.** In SOSP 1991.
  [Link](http://dx.doi.org/10.1145/121132.121137)
  - Hagmann, R. **Reimplementing the Cedar file system using logging and
  group commit.** In SOSP 1987. [Link](http://dx.doi.org/10.1145/41457.37518)
  - Roselli, Drew, Jacob R. Lorch, and Thomas E. Anderson. **A
  comparison of file system workloads.** In USENIX ATC, 2000.
  [Link](http://www.usenix.org/publications/library/proceedings/usenix2000/general/roselli.html)
  - Quinlan, Sean, and Sean Dorward. **Venti: A New Approach to Archival Storage.**
  In USENIX FAST, 2002. [Link](http://usenix.org/events/fast02/quinlan.html)
  - S. Ghemawat, H. Gobioff, and S.-T. Leung. **The google file system.** SIGOPS
  Oper. Syst. Rev., 37(5):29-43, 2003.
  - Policroniades, Calicrates, and Ian Pratt. **Alternatives for detecting
  redundancy in storage systems data.** In USENIX ATC, 2004.
  [Link](http://www.usenix.org/events/usenix04/tech/general/policroniades.html)
  - Prabhakaran, Vijayan, Andrea C. Arpaci-Dusseau, and Remzi H. Arpaci-Dusseau.
  **Analysis and evolution of journaling file systems.** In USENIX ATC, 2005.
  [Link](http://www.usenix.org/events/usenix05/tech/general/prabhakaran.html)
  - Nightingale, Edmund B., Kaushik Veeraraghavan, Peter
  M. Chen, and Jason Flinn. **Rethink the sync.** In OSDI 2006.
  [Link](http://dx.doi.org/10.1145/1394441.1394442)
  - Condit, Jeremy, Edmund B. Nightingale, Christopher Frost, Engin
  Ipek, Benjamin Lee, Doug Burger, and Derrick Coetzee. **Better
  I/O through byte-addressable, persistent memory.** In SOSP 2009.
  [Link](http://dx.doi.org/10.1145/1629575.1629589)
  - Harter, Tyler, Chris Dragga, Michael Vaughn, Andrea C. Arpaci-Dusseau,
  and Remzi H. Arpaci-Dusseau. **A file is not a file: understanding
  the I/O behavior of Apple desktop applications.** In SOSP 2011.
  [Link](http://dx.doi.org/10.1145/2043556.2043564)

#### Reliability

  - N. P. Kronenberg, H. M. Levy, and W. D. Strecker. **Vaxcluster: a
  closely-coupled distributed system.** ACM Trans. Comput. Syst., 4(2):130-146,
  1986.
  - J. Gray and D. P. Siewiorek. **High-availability computer systems.** Computer,
  24(9):39-48, 1991.
  - R. Wahbe, S. Lucco, T. E. Anderson, and S. L. Graham. **Efficient
  software-based fault isolation.** In SOSP 1993.
  - D. Engler, D. Y. Chen, S. Hallem, A. Chou, and B. Chelf. **Bugs as deviant
  behavior: a general approach to inferring errors in systems code.** SIGOPS
  Oper. Syst. Rev., 35(5):57-72, 2001.
  - M. M. Swift, B. N. Bershad, and H. M. Levy. **Improving the reliability
  of commodity operating systems.** In SOSP 2003.
  - F. Qin, J. Tucek, J. Sundaresan, and Y. Zhou. **Rx: treating bugs
  as allergies-a safe method to survive software failures.** In SOSP 2005.

#### Virtualization

  - C. A. Waldspurger. **Memory resource management in vmware esx server.**SIGOPS
  Oper.  Syst. Rev., 36(SI):181-194, 2002.
  - Barham, Paul, Boris Dragovic, Keir Fraser, Steven Hand, Tim Harris, Alex
  Ho, Rolf Neugebauer, Ian Pratt, and Andrew Warfield. **Xen and the art
  of virtualization.** In SOSP 2003. [Link](http://dx.doi.org/10.1145/945445.945462)
  - Adams, Keith, and Ole Agesen. **A comparison of software and hardware
  techniques for x86 virtualization.** In ASPLOS 2006.
  [Link](http://dx.doi.org/10.1145/1168857.1168860)
  - Chen, Xiaoxin, Tal Garfinkel, E. Christopher Lewis, Pratap
  Subrahmanyam, Carl A. Waldspurger, Dan Boneh, Jeffrey Dwoskin, and
  Dan R.K. Ports. **Overshadow: a virtualization-based approach to
  retrofitting protection in commodity operating systems.** In ASPLOS 2008.
  [Link](http://dx.doi.org/10.1145/1346281.1346284)


#### Architecture

  - Saavedra, R.H., and A.J. Smith. **Measuring cache and TLB performance and
    their effect on benchmark runtimes.** IEEE Transactions on Computers 44,
    no. 10 (1995): 1223-1235. [Link](http://dx.doi.org/10.1109/12.467697)
  - Nellans, David, Rajeev Balasubramonian, and Erik Brunvand. **A Case for
  Increased Operating System Support in Chip Multi-Processors.** In Proc. 2nd
  IBM Watson Conference on Interaction between Architecture, Circuits,
  and Compilers (PAC2), 2005. [Link](http://david.nellans.org/files/PAC2-2005.pdf)
  - Baumann, Andrew, Paul Barham, Pierre-Evariste Dagand, Tim Harris,
  Rebecca Isaacs, Simon Peter, Timothy Roscoe, Adrian Schuepbach, and Akhilesh
  Singhania. **The multikernel: a new OS architecture for scalable multicore
  systems.** In SOSP 2009. [Link](http://dx.doi.org/10.1145/1629575.1629579)
  - Boyd-Wickizer, Silas, Austin T. Clements, Yandong Mao,
  Aleksey Pesterev, M. Frans Kaashoek, Robert Morris, and Nickolai
  Zeldovich. **An analysis of Linux scalability to many cores.** In OSDI 2010.
  [Link](http://www.usenix.org/events/osdi10/tech/full_papers/Boyd-Wickizer.pdf)
  - Denning, Peter J. **The locality principle.** Commun. ACM 48, no. 7 (2005):
  19-24. [Link](http://dx.doi.org/10.1145/1070838.1070856)
  - Aho, Alfred V., Peter J. Denning, and Jeffrey D. Ullman. **Principles of
  Optimal Page Replacement.** Journal of the ACM 18, no. 1 (1971): 80-93.
  [Link](http://dx.doi.org/10.1145/321623.321632)

#### Energy

 - Narayanan, Dushyanth, Austin Donnelly, and Antony Rowstron. **Write
 off-loading: practical power management for enterprise storage.** In USENIX
 FAST 2008. [Link](http://www.usenix.org/events/fast08/tech/narayanan.html)
