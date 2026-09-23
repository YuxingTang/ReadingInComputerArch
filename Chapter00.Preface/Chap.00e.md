# Preface

*In theory, theory and practice are the same.*

*In practice, they’re different.*

*--Attribution unknown*

We are pleased to present you with this collection of readings in computer architecture. Computer architecture can be considered the science and art of selecting and interconnecting hardware components to create a computer that meets functional, performance, and cost goals. Computer architecture has a rich history of practice whose mastery aids those interested in moving the ﬁeld forward.

Computer architecture continues to ﬂourish in a dynamic environment. One can think of it as an interface with hardware implementation technologies below and application and system software above. From below exponential semiconductor advances continue to provide a greater number of faster transistors, while at the same time altering trade-offs both on and between chips (e.g., on-chip wires can exceed gate delays). Architects use the additional transistors in new ways to make the compound growth of a computer system’s capacity exceed the rate at which transistors are getting faster.

From above the applications that use architectures change. Classic applications have evolved, while new application domains emerge as advances make computing cost-effective to new areas. Application areas include scientiﬁc computing (e.g., quantum dynamics to vehicle crash simulation), business data processing (e.g., accounting to data mining), personal productivity tools (e.g., word processing and spreadsheets), global connectivity (e.g., electronic mail to telecommuting), and emerging applications (e.g., personal digital assistants to virtual reality).

It is to the students, researchers, and practitioners of this dynamic ﬁeld of computer architecture that we offer this new collection of readings. The rest of this preface discusses why are we introducing this reader now, how we select and introduce papers, and how one might use this reader.


## Why have a computer architecture reader now?

Why a reader now when the last widely successful readers are decades old, there are good textbooks in the ﬁeld, and the web is expanding as a source for technical information? In our view, the last widely-successful collection of readings in computer architecture was from Bell and Newell in 1971 [2] updated by Siewiorek in 1982 [6]. These readers were excellent, but much has happened since they were published and they are currently out of print.

Another reader than complements this one is the recently-published collection of selected papers from the ﬁrst 25 years of the International Symposium on Computer Architecture (ISCA) edited by Sohi in 1998 [8]. Our reader differs from the ISCA reader in two important ways. First, we have drawn papers from many sources, not just ISCA. Second, we have contributed introductory material that puts multiple papers in context. In contrast, the ISCA reader has an introduction by each of the original author sets that describes how their paper came about, and, sometimes, their view on its future impact. We ﬁnd these insights valuable and recommend the ISCA reader to you as well.

Why have a reader when there are good textbooks in the ﬁeld? The answer to this question has deep roots in the history of science. A primary source is a work written by the people who did the work (discovery, invention, etc.) when they did it. A secondary source is a work written after the primary source and usually by different authors. Textbooks are the vehicle usually used to introduce people to scientiﬁc or engineering disciplines. Textbooks are secondary sources designed for teaching. They summarize, synthesize, and interpret work in the intellectual model (paradigm) that prevails when they are written. They are indispensable for giving people a reasonablyrecent snapshot of the state of the art. Noteworthy textbooks in computer architecture include Almasi and Gottlieb [1], Culler and Singh with Gupta [3], Hennessy and Patterson [4], and Stone [9].

Students and professionals who wish to contribute to advancing a ﬁeld, however, must eventually move beyond textbooks to primary sources. New primary sources provide the most recent thinking that will not appear in textbooks for one or more years. In computer architecture, the pressure on “bleeding edge” primary sources is so great that conference papers have more impact on intellectual progress than do journal papers (which have a longer delay between submission and appearance).

This reader contains many old primary sources. We see three reasons people should read both old and new primary sources. First, experience reading old primary sources trains people to read new ones. Second, it makes them aware of the process of discovering. Primary sources over time give a “motion picture” rather than a “snap shot,” enabling people to see how the movie moves forward, and how they might continue moving the movie forward. Third, it allows people to see ideas in the context of multiple paradigms, instead of just within the current paradigm. Students in the early 1980’s, for example, could take whole courses on computer architecture with no mention of out-of-order execution, which was previously invented but out of vogue. We encourage readers interested in how science advances to read the landmark primary source by Thomas Kuhn [5].

Why have a reader when people can just use the web? The primary value of this reader is our editorial judgement for selecting papers (discussed in the following section) and background we provide in introducing them and placing them in context. This value is not diminished by the existence of the World Wide Web.

A secondary value of a reader is the coalescing into one hardcopy the hardcopies of many papers that are time-consuming to obtain. This value remains present for the many pre-web papers we have selected (and is enhanced as post-web researchers are more reluctant to do the leg work to obtain hardcopies the traditional way). This value is signiﬁcantly diminished, however, for postweb papers.

Our approach is to turn the web into an advantage. This hardcopy reader is supplemented with a web component at URL http://www.mkp.com/architecture-readings. This web page is modeled after this hardcopy reader. Its ten sections follow the ten chapters of this reader. Each contain pointers to recent papers and some text to introduce them and put them in context. In particular, the papers represent the best of the state of the art (e.g., a new microprocessor paper). Furthermore, our web component will be updated, at least, annually to make this reader a living document that responds more rapidly than is possible with hardcopy editions. Thus, the web allows us to dynamically extend the value our editorial judgement in a way not practical before the web.

## What’s in this reader and why?

This reader focuses on what we believe are and will continue to be the critical issues facing computer architects. These include issues in technology, evaluation methods, instruction set design, instruction level parallelism, dataﬂow/multithreading, memory systems, input/output systems, single-instruction multiple data parallelism, and multiple-instruction multiple data parallelism. The downside of our focus, however, is that many important areas of inquiry are not covered (e.g., computer aided design and computer arithmetic).

After limiting our focus, we were still left with the challenging task of selecting 50-odd papers to ﬁll a single bound volume from thousands of computer architecture papers that have been published. (For this reason, we hope that you do not judge us too harshly if we left our a few of your favorite papers). We have used several guidelines for selecting papers. We looked for papers that:

  *  were primary sources,
  * had lasting impact,
  * are readable,
  * are an illustrative case study, and
  * contribute insight to the modern reader.

We did not apply these guidelines rigidly, in part, because they can contradict each other (e.g., some primary sources are inscrutable to the modern reader). One consequence of our emphasis on primary sources is that we exclude most survey papers, including inﬂuential ones (e.g., Smith’s cache survey [7]).

Furthermore, the papers selected (and our original material) underwent two rounds of thoughtful review and discussion with a dozen researchers from academia and industry. They—see the acknowledgements at the end of this preface—helped to reﬁne the selected papers to even better reﬂect what the community considers important as well as what students should know. We, the editors, resolved conﬂicts in the advice we got and accept all responsibility for the ﬁnal version.

We have organized this reader into ten chapters. Each chapter is introduced by some original material that places the papers in context, introducing each, and, in many cases, providing additional content pertinent to the chapter topic but beyond the scope of the included papers.

Chapter 1, *Technology, Implementation, and Economics: Classic Machines*, examines the foundation of computer architecture. This chapter begins a discussion of classic machines from the ENIAC to Cray’s machines with an emphasis on how implementation technologies inﬂuenced architecture. It then discusses Gordon Moore’s amazing technology predictions from the 1960s (e.g., Moore’s Law) and how they lead to microprocessor-based computers. Included papers discuss IBM 360, CDC 6600, Cray 1, Moore’s predictions, and early Intel microprocessors.

Chapter 2, *Methods*, is not about computer architecture, per se, but about some of the methods used to evaluate and reﬁne architectures. Without these methods, the progress we have come to expect would not be possible. This chapter discusses the scientiﬁc method and the three major classes of methods that computer architects use: analytic modeling, simulation, and system monitoring. Techniques are illustrated with case studies from memory hierarchy evaluation. Included papers present Amdahl’s Law, a system monitoring study of the VAX-11/780, and trace-driven simulation algorithms of evaluating alternative caches.

Chapter 3, *Instruction Sets*, discusses the interface between software and hardware that is so critical in that it enables software to run on multiple generations of hardware. This chapter examines how changes in implementation technology and compiler technology have changed instruction set trade-offs, including putting the Complex Instruction Set Computer (CISC) versus Reduced Instruction Set Computer (RISC) debate of the 1980s into perspective. Included papers discuss instruction sets as compiler targets, RISC, CISC, the most widely-used instruction set (Intel 80386), and ideas on predication pertinent to emerging instruction sets such as IA-64.

Chapter 4, *Instruction Level Parallelism (ILP)*, examines issues critical to accelerating processor execution rates beyond the increases provided by faster transistors and bit-level parallelism. This chapter examines issues for executing multiple instructions in parallel that include hazards, precise exceptions, speculative execution, branch prediction, and explicitly parallel architectures such as Very Long Instruction Word (VLIW). Included papers discuss the IBM 360/91, IBM RS/ 6000, MIPS R10000, branch prediction, precise exceptions, out-of-order execution, and a survey of instruction level parallelism issues.

Chapter 5, *Dataﬂow and Multithreading*, discusses these two approaches to increasing parallelism and tolerating latency. Dataﬂow has had considerable intellectual impact, while multithreading appears poised to signiﬁcantly impact practice. This chapter explains how classic data ﬂow pushes limits by eliminating the program counter, while multithreading more conventionally (and more practically) uses multiple program counters. Included papers discuss foundational dataﬂow work, the tagged-token dataﬂow approach, an early multithreaded computer (HEP), and recent simultaneous multithreading ideas.

Chapter 6, *Memory Systems*, examines caches and virtual memory, two critical aspects of the memory hierarchy that can largely determine sustained computer performance. Since textbooks cover both concepts in detail, both discussions begin with early concepts and then examine selected more-recent concepts that are illustrated in the papers. Included cache papers discuss the ﬁrst data cache proposals (Wilkes), the ﬁrst commercial cache (IBM 380/85), non-blocking caches, snooping cache coherence, and victim caches/stream buffers. Included virtual memory papers present the ﬁrst virtual memory system (Atlas), a 1980s virtual memory system from VAX-11/780, and a case study of some of the interactions between caches and virtual memory.

Chapter 7, *I/O: Storage Systems, Networks*, and Graphics, examines systems needed to make computers interact with the outside world. In particular, this chapter discusses Input/Output (I/O) economics, presents a case study of Personal Computer (PC) I/O systems options, and introduces included papers. Included papers discuss historical I/O systems, disk modeling, Redundant Arrays of Inexpensive Disks (RAID), Ethernet local area network, routing in interconnection networks, and a case study of graphics support.

Chapter 8, *Single-Instruction Multiple Data (SIMD) Parallelism*, discusses an approach to parallel computing where a single thread of control directs the manipulation of multiple data operations. SIMD’s perceived utility has waxed and waned several times over the past decades. Even though interest in SIMD has currently waned readers should be familiar with SIMD because it is likely to wax again in the future. This chapter introduces basic SIMD concepts and included papers. Included papers discuss the original SIMD/MIMD taxonomy, a SIMD machine that issued dependent operations (BSP), and massive processing in memory.

Chapter 9, *Multiprocessors and Multicomputers*, discusses computing systems in which multiple processors can operate independently. This is called multiple-instruction multiple data (MIMD) parallelism. Such systems have long been the “future” of computing, but now they have ﬁnally earned substantial commercial success. This chapter discusses parallel software, shared memory multiprocessors (processors joined via the memory system), multicomputers (processors joined via the I/O system), and selected future trends. Included shared memory multiprocessor papers discuss an early prototype machine (CMU C.mmp), memory consistency models, cache coherence, a recent scalable example (Stanford DASH), and a cache-only memory architecture (COMA). Multicomputer papers discuss an early multicomputer (Caltech Cosmic Cube) and shared memory on multicomputers.

Finally, Chapter 10, *Recent Implementations and Future Prospects*, revisits for modern machines some of the issues Chapter 1 raised for classic machines. Included papers discuss Intel Pentium, Intel Pentium Pro, and future microprocessor trends.


## How to use this book

We have prepared this book with the hope that it will be valuable to both professionals and students. Both groups can read these primary sources and our original commentary to learn more about the rich history of computer architecture and to better see how to move the ﬁeld forward.

For instructors, we see three beneﬁcial ways to use our reader. First, it can be used as a supplement to a primary textbook. At Wisconsin, for example, we have a primary graduate architecture course that focuses on uniprocessors. Most recent instructors have used Hennessy and Patterson [4] and a custom reader of papers. We anticipate some instructors will use this reader and some very recent papers from the web (at this reader’s web site and elsewhere) to replace their current custom reader. Wisconsin also has a secondary graduate architecture course that focuses on parallel processing. It has recently used Culler and Singh with Gupta [3] and a custom reader. As above, one could replace the custom reader with this reader and supplementary web papers.

A second approach ﬁts schools that have a ﬁrst course that uses a textbook and a second course whose orientation toward projects or case studies favors a custom reader. Once again this reader and web papers could replace the custom reader.

A third model is a graduate course or courses that eschew textbooks and just use readers (as occurs for some offerings of Wisconsin’s courses). This approach requires considerable skill and effort from faculty to tie disjoint ideas together. Using this reader and selected other papers, possibly web only, will ease the instructor’s burden since we make considerable effort to tie our included papers together.

## Acknowledgments

We wish to thank the many people that have contributed ideas, comments and criticism to this reader, especially the anonymous referees recruited by Morgan Kaufmann, Arvind, Matt Farrens, Jim Goodman, Rishiyur Nikhil, Daniel Sorin, Jim Smith, and David Wood. Of course, all responsibility for what is said or included in this reader falls on us.

We also want thank the wonderful staff at Morgan Kaufmann for their talents, efforts, and encouragement, especially Denise Penrose, Meghan Keeffe, and Marilyn Alan.

## References

[1]. G. S. Almasi and A. Gottlieb. Highly Parallel Computing. Benjamin / Cummings Publishing Company, Inc., 1994.
[2]. C. G. Bell and A. Newell. Computer Structures: Readings and Examples. McGraw Hill, 1971.
[3]. David Culler, J. P. Singh, and Anoop Gupta. Parallel Computer Architecture: A Hardware/Software Approach. Morgan Kaufmann, 1998.
[4]. John L. Hennessy and David A. Patterson. Computer Architecture: A Quantitative Approach. Morgan Kaufmann, second edition, 1996.
[5]. T. S. Kuhn. The Structure of Scientific Revolutions. Univ. of Chicago Press, second edition, 1970.
[6]. D. P. Siewiorek, C. G. Bell, and A. Newell. Computer Structures: Principles and Examples. McGraw Hill, 1982.
[7]. Alan J. Smith. Cache Memories. ACM Computing Surveys, 14(3):473–530, 1982.
[8]. Gurindar Sohi. 25 Years of the International Symposia on Computer Architecture: Selected Papers. ACM Press, 1998.
[9]. Harold S. Stone. High-Performance Computer Architecture. Addison Wesley, third edition, 1993.

## Included Papers

### Chapter 1: Classic Machines: Technology, Implementation, and Economics
G. M. Amdahl, G. A. Blaauw, and F. P. Brooks, Jr., "Architecture of the IBM System/360," *IBM Journal of Research and Development*, Apr. 1964.

J. E. Thornton, "Parallel operation in the Control Data 6600," *Fall Joint Computers Conference*, vol. 26, pp. 33-40, 1961.

R. M. Russell, "The CRAY-1 computer system," *Communications of the ACM*, 21(1):63-72, 1978.

J. S. Kolodzey, "CRAX-1 computer technology," *IEEE Transactions on Components, Hybrids, and Manufacturing Technology*, CHMT-4(2), PP. 181-187, June 1981.

G. E. Moore, "Cramming more components onto integrated circuits," Electronics, pp. 114-117, Apr. 1965.

S. Mazor, "The history of the microcomputer--Invention and evolution," *Proceedings of the IEEE*, PP. 1601-1607, Dec. 1995.

### Chapter 2: Methods
G. M. Amdahl, "Validity of the single processor approach to achieving large scale computing capabilities," *AFIPS Conference Proceedings*, PP. 483-485, Apr. 1967.

M. D. Hill and A. J. Smith, "Evaluating associativity in CPU caches," *IEEE Transactions on Computers*, C-38(12):1612-1630, 1989.

J. S. Emer and D. W. Clark, "A characterization of processor performance in the VAX-11/780," *Proceedings of the Eleventh International Symposium on Computer Architecture*, Ann Arbor, MI, PP. 301-310, June 1984.

### Chapter 3: Instruction Sets
W. A. Wulf, "Compilers and computer architecture," *IEEE Computer*, 14(7):41-48, 1981.

G. Radin, "The 801 minicomputer," *Proceedings of the Symposium on Architectural Support for Programming Languages and Operating Systems*, рр. 39-47, Маг. 1982.

D. A. Patterson and D. R. Ditzel, "The case for the reduced instruction set computer," *ACM Computer Architecture News*, 8(6):25-33, 15 Oct. 1980.

R. P. Colwell, C. Y. Hitchcock III, E. D. Jensen, H. M. Brinkley Sprunt, and C. P. Kollar, "Instruction Sets and Beyond: Computers, complexity, and controversy," *IEEE Computer*, 18(9), 1985.

J. Crawford, "Architecture of the Intel 80386," *Proceedings of the ICCD*, pp. 155-160, Oct. 1986.

S. A. Mahlke, R. E. Hank, J. E. McCormick, D. I. August, and W. W. Hwu, "A comparison of full and partial predicated execution support for ILP processors," *Proceedings of the 22nd Annual Symposium on Computer Architecture* pp. 138-150, June 1995.

### Chapter 4: Instruction Level Parallelism (ILP)
D. W. Anderson, F. J. Sparacio, and R. M. Tomasulo, "The IBM System/360 model 91: Machine philosophy and instruction-handling," *IBM Journal of Research and Development*, Jan. 1967.

J. E. Smith and A. R. Pleszkun, "Implementing precise interrupts in pipelined processors," *IEEE Transactions on Computers*, C-37(5):562-573, May 1988.

J. E. Smith, "A study of branch prediction strategies," *Proceedings of the Eighth Annual Symposium on Computer Architecture*, pp. 135-148, May 1981.

T.-Y. Yeh and X. N. Patt, "Two-level adaptive training
branch prediction," *Proceedings of the 24th Annual Workshop on Microprogramming (MICRO-24)*, Albuquerque, NM, PP. 55-60, Dec. 1991.

Y N. Patt, W. W. Hwu, and M. Shebanow, "HPS, a new microarchitecture: Introduction and rationale," *Proceedings of the 18th Annual Workshop on Microprogramming*, Pacific Grove, CA, PP. 103-108, Dec. 1985.

G. S. Sohi and S. Vajapeyam, "Instruction issue logic for high-performance, interruptable pipelined proces-sors," *Proceedings of the 14th Annual Symposium o Computer Architecture* Pp. 27-34, June 1987.

G. F. Grohoski, "Machine organization of the IBM RISC System/6000 processor," *IBM Journal of Research and Development*, 34(1):37-58, 1990.

K. C. Yeager "The Mips R10000 superscalar microproces-sor," *IEEE Micro*, 16(2):2840, 1996.

B. R. Rau and J. A. Fisher, "Instruction-level parallel processing; History, overview, and perspective," *The Journal of Supercomputing*, 7(1):9-50, 1993. Reprinted in Rau and Fisher (eds.), *Instruction-Level Parallelism*, Norwell, MA: Kluwer, 1993.

### Chapter 5: Dataflow and Multithreading
J. B. Dennis and D. P. Misunas, "A preliminary architect for a basic data-flow processor," *Proceedings of the 2nd Annual Symposium on Computer Architecture, Computer Architecture News*, 3(4): 126-132, 1974.

Arvind and R. S. Nikhil. "Executing a program on the MIT tagged-token dataflow architecture," *IEEE Transactions on Computers*, 39(3):300-318, 1990

B. J. Smith, "Architecture and applications of the HEP multiprocessor computer system," *Proceedings of the International Society for Optical Engineering*, 241-248, 1981.

D. M. Tullsen, S. J. Eggers, J. S. Emer, H. M. Levy, J. L. Lo, and R. L. Stamm, "Exploiting choice: Instruction fetch and issue on an implementable simultaneous multithreading processor," *Proceedings of the 23rd Annual Symposium on Computer Architecture*, pp. 191-202, May 1996.

### Chapter 6: Memory Systems
M. V. Wilkes, "Slave memories and dynamic storage allo-cation," *IEEE Transactions on Electronic Computers*, EC-14(2):270-271, 1965.

J. S. Liptay, "Structural aspects of the System/360 model 85, part II: The cache," *IBM Systems Journal*, 7(1): 15-21, 1968.

D. Kroft, "Lockup-free instruction fetch/prefetch cache organization," *Proceedings of the Eighth Symposium on Computer Architecture* pp. 81-87, May 1981.

J. R. Goodman, "Using cache memory to reduce processor-memory traffic," *Proceedings of the Tenth International Symposium on Computer Architecture*, Stockholm, Sweden, pp. 124-131, June 1983.

N. P. Jouppi, "Improving direct-mapped cache performance by the addition of a small fully-associative cache and prefetch buffers," *Proceedings of the 17th Annual Symposium on Computer Architecture, Computer Architecture News*, 18(2):364-373, 1990

T. Kilburn, D. B. G. Edwards, M. J. Lanigan, and F. H. Sumner, "One-level storage system," *IRE Transactions*, EC-11(2):223-235, 1962.

D. W. Clark and J. S. Emer, "Performance of the VAX-11/780 translation buffer: Simulation and measurement," *ACM Transactions on Computer Systems*, 3(1):31-62, 1985.

W.-H. Wang, J.-L. Baer, and H. M. Levy, "Organization and performance of a two-level virtual-real cache hierarchy," *Proceedings of the 16th Annual International Symposium on Computer Architecture*, Jerusalem, pp. 140-148, June 1989.

### Chapter 7: I/0: Storage Systems, Networks, and Graphics
M. Smotherman, "A sequencing-based taxonomy of 1/O systems and review of historical machines," *ACM Computer Architecture News* 17(5):5-15, Sept. 1989.

*Storage Systems*

C. Ruemmler and J. Wilkes, "An introduction to disk drive modeling," *IEEE Computer* 27(3): 17-28, 1994.

D. A. Patterson, G. Gibson, and R. H. Katz, "A case for redundant arrays of inexpensive disks (RAID)." *Proceedings of the ACM SIGMOD Conference*, Chicago, IL, June 1988.

*Netorks*

R. M. Metcalfe and D. R. Boggs, "Ethernet: Distributed packet switching for local computer networks." *Communications of the ACM*, 19(7):395-404.

L. M. Ni and P. K. McKinley, "A survey of wormhole routing techniques in direct networks," *IEEE Computer*, 26(2):62-76, 1993.

*Graphics*

K. Akeley, "Reality engine graphics," *SIGGRAPH '93 Proceedings*, pp. 109-116.

### Chapter 8: Single-Instruction Multiple Data (SIMD) Parallelism
M. J. Flynn, " Very high-speed computing systems," *Proceedings of the IEEE*, vol. 54, no. 12, Dec. 1966.

D. J. Kuck and R. A. Stokes, "The Burroughs scientific processor (BSP)," *IEEE Transactions on Computers*, C-31(5):363-376, 1982.

M. Gokhale, B. Holmes, and K. lobst, "Processing in mem-ory: The Terasys massively parallel PIM array," *IEEE Computer*, 28(4):23-31, 1995.

### Chapter 9: Multiprocessors and Multicomputers
W. A. Wulf and S. P. Harbison, "Reflections in a pool of processors/An experience report on C.mmp/Hydra," *Proceedings of the National Computer Conference (AFIPS)*, June 1978.

L. Lamport, "How to make a multiprocessor computer that correctly executes multiprocess programs," *IEEE Transactions on Computers*, C-28(9):690-691, 1979.

L. M. Censier and P. Feautrier, "A new solution to coherence problems in multicache systems," *IEEE Transactions on Computers*, C-27(12):1112-1118, 1978.

D. Lenoski, J. Laudon, K. Gharachorloo, W.-D. Weber, A. Gupta, J. Hennessy, M. Horowitz, and M. Lam, "The Stanford Dash multiprocessor," *IEEE Computer*, 25(3):63-79, 1992.

E. Hagersten, A. Landin, and S. Haridi "DDM—A cache-only memory architecture," *IEEE Computer*, 25(9):44-54, 1992.

C. L. Seitz, "The cosmic cube," *Communications of the ACM*, pp. 22-33, Jan. 1985.

K. Li and P. Hudak, "Memory coherence in shared virtual memory systems," *ACM Transactions on Computer Systems*, 7(4):321-359, 1989.

### Chapter 10: Recent Implementations and Future Prospects
D. Alpert and D. Avnon, "Architecture of the Pentium microprocessor," *IEEE Micro*, 13(3):11-21, 1993.

D. Papworth, "Tuning the Pentium Pro microarchitecture," *IEEE Micro*, 16(2):8-15, 1996.

M. Slater, "The microprocessor today," *IEEE Micro*, 16(6):32-44, 1996.

A. Yu, "The future of microprocessors," *IEEE Micro*, 16(6):46-53, 1996.

