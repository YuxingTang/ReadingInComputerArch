# 前言

*理论上，理论与实践是一致的。*

*实际上，它们并不相同。*

*——出处不详*

&emsp;&emsp; 我们很高兴向您呈现这套计算机体系结构方面的读物。计算机体系结构可以被视为选择和互连硬件组件以创建满足功能、性能和成本目标的计算机的科学和艺术。计算机体系结构拥有丰富的实践历史，其精通有助于那些有兴趣推动该领域发展的人们。

&emsp;&emsp; 计算机体系结构在一个动态的环境中持续蓬勃发展。我们可以将其视为一个接口，其下方是硬件实现技术，上方是应用和系统软件。从下方来看，半导体技术的指数级进步不断提供更多更快的晶体管，同时也改变了芯片内部和芯片之间的权衡（例如，芯片上的导线延迟可能超过门延迟）。架构师以新的方式使用额外的晶体管，使计算机系统容量的复合增长速度超过晶体管速度的提升速度。

&emsp;&emsp; 从上方来看，使用架构的应用也在不断变化。经典应用不断发展演进，随着计算技术的进步，新的应用领域也随之涌现，使得计算成本在新的领域得以降低。这些应用领域包括科学计算（例如，从量子动力学到车辆碰撞模拟）、商业数据处理（例如，从会计到数据挖掘）、个人效率工具（例如，文字处理和电子表格）、全球互联（例如，从电子邮件到远程办公）以及新兴应用（例如，从个人数字助理到虚拟现实）。

&emsp;&emsp; 我们向计算机体系结构这一充满活力的领域中的学生、研究人员和从业人员推荐这本全新的文集。本序言的其余部分将探讨我们为何选择此时推出这本文集、我们如何选择和介绍文章，以及读者如何使用这本文集。

## 为什么现在需要计算机体系结构读本？

&emsp;&emsp; 为什么现在需要一本读本呢？毕竟，上一部广受欢迎的读本已经是几十年前的事了，而且该领域已经有很多优秀的教科书，网络也正在迅速发展成为技术信息的重要来源。我们认为，上一部广受欢迎的计算机体系结构读本是贝尔和纽厄尔于1971年出版的[2]，并由西维奥雷克于1982年进行了更新[6]。这些读本非常优秀，但自出版以来，计算机体系结构领域发生了翻天覆地的变化，而且目前已经绝版。

&emsp;&emsp; 另一部可以与本书互补的读本是索希于1998年编辑出版的国际计算机体系结构研讨会（ISCA）前25年论文选集[8]。我们的读本与ISCA读本有两个重要的区别。首先，我们收录的论文来自多个来源，而不仅仅是ISCA。其次，我们添加了导论性材料，将多篇论文置于特定的背景之中。相比之下，ISCA 读本的每位作者都撰写了导言，阐述了论文的创作缘起，有时还会展望其未来影响。我们认为这些见解弥足珍贵，因此也向您推荐 ISCA 读本。

&emsp;&emsp; 既然该领域已有优秀的教科书，为何还要出版读本呢？这个问题的答案深深植根于科学史。一手资料是指由从事相关工作（发现、发明等）的人在当时撰写的作品。二手资料是指在一手资料之后撰写的作品，通常由不同的作者撰写。教科书通常是向人们介绍科学或工程学科的载体。教科书是为教学而设计的二手资料。它们根据撰写时盛行的知识模式（范式）对相关工作进行总结、综合和阐释。对于让人们了解该领域的最新进展而言，教科书不可或缺。计算机体系结构领域值得关注的教科书包括 Almasi 和 Gottlieb 的著作 [1]、Culler 和 Singh 与 Gupta 合著的著作 [3]、Hennessy 和 Patterson 的著作 [4] 以及 Stone 的著作 [9]。

&emsp;&emsp; 然而，希望为某一领域发展做出贡献的学生和专业人士最终必须超越教科书，转向一手资料。最新的一手资料提供了最新的研究成果，这些成果在教科书中可能要过一年或更长时间才会出现。在计算机体系结构领域，“前沿”一手资料的需求如此之大，以至于会议论文对学术进步的影响甚至超过了期刊论文（期刊论文从投稿到发表的周期更长）。

&emsp;&emsp; 本书收录了许多早期的一手资料。我们认为人们应该阅读新旧一手资料的原因有三点。首先，阅读早期一手资料的经验能够帮助人们更好地阅读新的资料。其次，它能让人们意识到发现的过程。随着时间的推移，一手资料呈现的是一部“动态影像”，而非“快照”，它使人们能够看到历史的进程，以及如何继续推动历史的发展。第三，它使人们能够从多种范式的角度看待各种想法，而不仅仅局限于当前的范式。例如，20世纪80年代初的学生可以学习完整的计算机体系结构课程，却完全不会提及乱序执行——尽管乱序执行技术此前已被发明，但当时已不再流行。我们鼓励对科学发展感兴趣的读者阅读托马斯·库恩的里程碑式著作[5]。

&emsp;&emsp; 既然人们可以直接使用网络，为什么还要出版这本选集呢？这本选集的主要价值在于我们对论文的选择（将在下一节讨论）以及我们在介绍论文并将其置于特定语境时提供的背景信息。即使有了万维网，这一价值也丝毫不减。

&emsp;&emsp; 这本选集的另一个价值在于，它将许多耗时费力获取的论文纸质版汇集到一本纸质书中。对于我们选择的众多互联网出现之前的论文而言，这一价值依然存在（并且随着互联网时代的研究人员越来越不愿意以传统方式获取纸质版，这一价值也得到了进一步提升）。然而，对于后网络时代的论文而言，这种价值已显著降低。

&emsp;&emsp; 我们的方法是将网络转化为优势。这本纸质读物配有一个位于网址 <http://www.mkp.com/architecture-readings> 网络组件。该网页以纸质读物为蓝本，分为十个部分，对应纸质读物的十个章节。每个部分都包含指向近期论文的链接，以及介绍这些论文并将其置于特定语境的文字说明。尤其值得一提的是，这些论文代表了当前技术的最高水平（例如，一篇关于微处理器的最新论文）。此外，我们的网络组件至少每年更新一次，使这本读物成为一个动态更新的文档，其响应速度远超纸质版本。因此，网络使我们能够以前所未有的方式动态地扩展我们编辑判断的价值。

## 这本选集包含哪些内容？为什么选择它？

&emsp;&emsp; 这本选集聚焦于我们认为计算机体系结构领域当前及未来将持续面临的关键问题。这些问题涵盖技术、评估方法、指令集设计、指令级并行性、数据流/多线程、内存系统、输入/输出系统、单指令多数据并行性以及多指令多数据并行性等方面。然而，这种聚焦方式的不足之处在于，许多重要的研究领域并未涉及（例如，计算机辅助设计和计算机算术）。

&emsp;&emsp; 即便缩小了聚焦范围，我们仍然面临着一项艰巨的任务：从已发表的数千篇计算机体系结构论文中，挑选出大约50篇论文来充实这本合订本。（因此，如果您发现我们遗漏了一些您喜爱的论文，敬请谅解。）我们在选择论文时遵循了一些指导原则。我们寻找的论文需满足以下条件：

  * 为一手资料，
  * 具有持久影响，
  * 易于阅读，
  * 具有案例研究意义，以及
  * 能为现代读者提供新的见解。

&emsp;&emsp; 我们并未严格遵循这些准则，部分原因是这些准则之间可能存在矛盾（例如，某些一手资料对现代读者而言晦涩难懂）。我们强调一手资料的一个后果是，我们排除了大多数综述性论文，包括一些具有影响力的论文（例如，Smith 的缓存调查 [7]）。

&emsp;&emsp; 此外，我们与来自学术界和工业界的十几位研究人员进行了两轮深入的审阅和讨论，最终选出的论文（以及我们自己的原创材料）也得到了他们的支持。他们——详见本序言末尾的致谢——帮助我们完善了选出的论文，使其更好地反映学术界认为重要的内容以及学生应该了解的知识。我们作为编辑，解决了收到的建议中的冲突，并对最终版本承担全部责任。

&emsp;&emsp; 本书共分为十章。每章开头都有原创内容，将各篇论文置于相应的背景之中，并对每篇论文进行介绍，而且在许多情况下，还会提供与本章主题相关但超出论文范围的补充信息。

&emsp;&emsp; 第一章“技术、实现与经济：经典机器”探讨了计算机体系结构的基础。本章首先讨论了从ENIAC到Cray机器的经典计算机，重点阐述了实现技术如何影响体系结构。随后，本章讨论了戈登·摩尔在20世纪60年代提出的惊人技术预测（例如摩尔定律），以及这些预测如何促成了基于微处理器的计算机的诞生。收录的论文讨论了IBM 360、CDC 6600、Cray 1、摩尔的预测以及早期的英特尔微处理器。

&emsp;&emsp; 第二章“方法”本身并非讨论计算机体系结构，而是介绍一些用于评估和改进体系结构的方法。如果没有这些方法，我们所期望的进步将无法实现。本章讨论了科学方法以及计算机架构师使用的三大类方法：分析建模、仿真和系统监控。文中通过内存层次结构评估的案例研究来阐述这些技术。收录的论文介绍了阿姆达尔定律、VAX-11/780 的系统监控研究以及用于评估替代缓存的跟踪驱动仿真算法。

&emsp;&emsp; 第三章“指令集”讨论了软件与硬件之间的接口，该接口至关重要，因为它使软件能够在多代硬件上运行。本章探讨了实现技术和编译器技术的变革如何改变了指令集的权衡取舍，包括对 20 世纪 80 年代复杂指令集计算机 (CISC) 与精简指令集计算机 (RISC) 之争的重新审视。收录的论文讨论了作为编译器目标的指令集、RISC、CISC、最广泛使用的指令集（Intel 80386）以及与新兴指令集（例如 IA-64）相关的谓词概念。

&emsp;&emsp; 第四章“指令级并行性 (ILP)”探讨了超越更快晶体管和位级并行性所带来的性能提升，对提高处理器执行速度至关重要的问题。本章研究了并行执行多条指令所面临的问题，包括冒险、精确异常、推测执行、分支预测以及显式并行架构（例如超长指令字 (VLIW)）。收录的论文讨论了 IBM 360/91、IBM RS/6000、MIPS R10000、分支预测、精确异常、乱序执行以及指令级并行性问题的概述。

&emsp;&emsp; 第五章“数据流与多线程”探讨了提高并行性和容忍延迟的两种方法。数据流理论已产生深远的理论影响，而多线程技术似乎有望对实践产生重大影响。本章解释了经典数据流如何通过消除程序计数器来突破性能极限，而多线程技术则更传统（也更实用）地使用多个程序计数器。本章收录的论文讨论了数据流的基础工作、标记令牌数据流方法、早期多线程计算机（高能物理）以及最新的同步多线程思想。

&emsp;&emsp; 第六章“内存系统”考察了缓存和虚拟内存，这是内存层次结构中两个至关重要的方面，它们在很大程度上决定了计算机的持续性能。由于教科书已对这两个概念进行了详细阐述，因此本章的讨论均从早期概念入手，然后探讨论文中阐述的选定的最新概念。本书收录的缓存论文探讨了最早的数据缓存方案（Wilkes）、第一个商用缓存（IBM 380/85）、非阻塞缓存、窥探式缓存一致性以及牺牲缓存/流缓冲区。收录的虚拟内存论文介绍了第一个虚拟内存系统（Atlas）、20 世纪 80 年代 VAX-11/780 的虚拟内存系统，以及缓存与虚拟内存交互的案例研究。

&emsp;&emsp; 第七章“I/O：存储系统、网络和图形”探讨了计算机与外部世界交互所需的系统。本章重点讨论了输入/输出 (I/O) 经济性，提供了个人计算机 (PC) I/O 系统选项的案例研究，并介绍了收录的论文。收录的论文讨论了历史 I/O 系统、磁盘建模、廉价磁盘冗余阵列 (RAID)、以太网局域网、互连网络中的路由以及图形支持的案例研究。

&emsp;&emsp; 第 8 章“单指令多数据 (SIMD) 并行”探讨了一种并行计算方法，其中单个控制线程负责指挥多个数据操作。过去几十年中，SIMD 的实用性几经起伏。尽管目前人们对 SIMD 的兴趣有所减弱，但读者仍应熟悉 SIMD，因为它未来很可能会再次流行起来。本章介绍了 SIMD 的基本概念和相关论文。这些论文讨论了最初的 SIMD/MIMD 分类、一种发出依赖操作的 SIMD 机器（BSP）以及内存中的大规模处理。

&emsp;&emsp; 第 9 章“多处理器和多计算机”讨论了多个处理器可以独立运行的计算系统。这被称为多指令多数据 (MIMD) 并行。此类系统长期以来一直被视为计算的“未来”，如今终于取得了巨大的商业成功。本章讨论并行软件、共享内存多处理器（通过内存系统连接的处理器）、多计算机（通过 I/O 系统连接的处理器）以及一些未来发展趋势。其中收录的共享内存多处理器论文探讨了早期原型机（CMU C.mmp）、内存一致性模型、缓存一致性、一个近期可扩展的示例（斯坦福 DASH）以及一种仅缓存内存架构（COMA）。多计算机论文则讨论了早期多计算机（加州理工学院 Cosmic Cube）以及多计算机上的共享内存。

&emsp;&emsp; 最后，第 10 章“近期实现与未来展望”重新审视了第 1 章针对经典机器提出的一些问题，并探讨了现代机器的相关问题。其中收录的论文讨论了英特尔奔腾处理器、英特尔奔腾 Pro 处理器以及未来微处理器的发展趋势。

## 如何使用本书

&emsp;&emsp; 我们希望本书能够对专业人士和学生都有价值。两组人员都可以阅读这些原始资料和我们原创的评论，以深入了解计算机体系结构的悠久历史，并更好地理解如何推动该领域的发展。

&emsp;&emsp; 对于教师而言，我们认为这本阅读器有三种有益的用途。首先，它可以作为主要教材的补充。例如，在威斯康星大学，我们开设了一门以单处理器为重点的计算机体系结构研究生课程。最近的大多数教师都使用 Hennessy 和 Patterson [4] 的著作以及一个定制的论文阅读器。我们预计一些教师会使用这本阅读器以及一些来自网络（包括本阅读器网站和其他网站）的最新论文来取代他们目前使用的定制阅读器。威斯康星大学还开设了一门以并行处理为重点的计算机体系结构研究生课程。该课程最近使用了 Culler 和 Singh 与 Gupta [3] 的著作以及一个定制阅读器。如上所述，教师可以用这本阅读器和补充的网络论文来取代他们目前使用的定制阅读器。

&emsp;&emsp; 第二种方法适用于第一门课程使用教科书，而第二门课程则侧重于项目或案例研究，更适合使用定制读物的学校。同样，本读物和网络论文可以替代定制读物。

&emsp;&emsp; 第三种模式是研究生课程，这些课程摒弃教科书，只使用读物（例如威斯康星大学的部分课程）。这种方法要求教师付出相当的技巧和努力，才能将看似无关的观点串联起来。使用本读物和精选的其他论文（可能仅限网络版）可以减轻教师的负担，因为我们已尽力将收录的论文联系起来。

## 致谢

&emsp;&emsp; 我们衷心感谢众多为本读物贡献想法、评论和批评的人士，尤其要感谢 Morgan Kaufmann、Arvind、Matt Farrens、Jim Goodman、Rishiyur Nikhil、Daniel Sorin、Jim Smith 和 David Wood 邀请的匿名审稿人。当然，本读物中所有内容的责任均由我们承担。

&emsp;&emsp; 我们还要感谢 Morgan Kaufmann 的优秀员工，感谢他们的才华、努力和鼓励，特别是 Denise Penrose、Meghan Keeffe 和 Marilyn Alan。

## 参考文献

[1]. G. S. Almasi and A. Gottlieb. Highly Parallel Computing. Benjamin / Cummings Publishing Company, Inc., 1994.
[2]. C. G. Bell and A. Newell. Computer Structures: Readings and Examples. McGraw Hill, 1971.
[3]. David Culler, J. P. Singh, and Anoop Gupta. Parallel Computer Architecture: A Hardware/Software Approach. Morgan Kaufmann, 1998.
[4]. John L. Hennessy and David A. Patterson. Computer Architecture: A Quantitative Approach. Morgan Kaufmann, second edition, 1996.
[5]. T. S. Kuhn. The Structure of Scientific Revolutions. Univ. of Chicago Press, second edition, 1970.
[6]. D. P. Siewiorek, C. G. Bell, and A. Newell. Computer Structures: Principles and Examples. McGraw Hill, 1982.
[7]. Alan J. Smith. Cache Memories. ACM Computing Surveys, 14(3):473–530, 1982.
[8]. Gurindar Sohi. 25 Years of the International Symposia on Computer Architecture: Selected Papers. ACM Press, 1998.
[9]. Harold S. Stone. High-Performance Computer Architecture. Addison Wesley, third edition, 1993.

## 包含的论文

### 第一章：经典机器：技术、实现和经济性
G. M. Amdahl, G. A. Blaauw, and F. P. Brooks, Jr., "Architecture of the IBM System/360," *IBM Journal of Research and Development*, Apr. 1964.

J. E. Thornton, "Parallel operation in the Control Data 6600," *Fall Joint Computers Conference*, vol. 26, pp. 33-40, 1961.

R. M. Russell, "The CRAY-1 computer system," *Communications of the ACM*, 21(1):63-72, 1978.

J. S. Kolodzey, "CRAX-1 computer technology," *IEEE Transactions on Components, Hybrids, and Manufacturing Technology*, CHMT-4(2), PP. 181-187, June 1981.

G. E. Moore, "Cramming more components onto integrated circuits," Electronics, pp. 114-117, Apr. 1965.

S. Mazor, "The history of the microcomputer--Invention and evolution," *Proceedings of the IEEE*, PP. 1601-1607, Dec. 1995.

### 第二章：方法
G. M. Amdahl, "Validity of the single processor approach to achieving large scale computing capabilities," *AFIPS Conference Proceedings*, PP. 483-485, Apr. 1967.

M. D. Hill and A. J. Smith, "Evaluating associativity in CPU caches," *IEEE Transactions on Computers*, C-38(12):1612-1630, 1989.

J. S. Emer and D. W. Clark, "A characterization of processor performance in the VAX-11/780," *Proceedings of the Eleventh International Symposium on Computer Architecture*, Ann Arbor, MI, PP. 301-310, June 1984.

### 第三章：指令集
W. A. Wulf, "Compilers and computer architecture," *IEEE Computer*, 14(7):41-48, 1981.

G. Radin, "The 801 minicomputer," *Proceedings of the Symposium on Architectural Support for Programming Languages and Operating Systems*, рр. 39-47, Маг. 1982.

D. A. Patterson and D. R. Ditzel, "The case for the reduced instruction set computer," *ACM Computer Architecture News*, 8(6):25-33, 15 Oct. 1980.

R. P. Colwell, C. Y. Hitchcock III, E. D. Jensen, H. M. Brinkley Sprunt, and C. P. Kollar, "Instruction Sets and Beyond: Computers, complexity, and controversy," *IEEE Computer*, 18(9), 1985.

J. Crawford, "Architecture of the Intel 80386," *Proceedings of the ICCD*, pp. 155-160, Oct. 1986.

S. A. Mahlke, R. E. Hank, J. E. McCormick, D. I. August, and W. W. Hwu, "A comparison of full and partial predicated execution support for ILP processors," *Proceedings of the 22nd Annual Symposium on Computer Architecture* pp. 138-150, June 1995.

### 第四章：指令级并行（ILP: Instruction Level Parallelism）
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

### 第五章：数据流
J. B. Dennis and D. P. Misunas, "A preliminary architect for a basic data-flow processor," *Proceedings of the 2nd Annual Symposium on Computer Architecture, Computer Architecture News*, 3(4): 126-132, 1974.

Arvind and R. S. Nikhil. "Executing a program on the MIT tagged-token dataflow architecture," *IEEE Transactions on Computers*, 39(3):300-318, 1990

B. J. Smith, "Architecture and applications of the HEP multiprocessor computer system," *Proceedings of the International Society for Optical Engineering*, 241-248, 1981.

D. M. Tullsen, S. J. Eggers, J. S. Emer, H. M. Levy, J. L. Lo, and R. L. Stamm, "Exploiting choice: Instruction fetch and issue on an implementable simultaneous multithreading processor," *Proceedings of the 23rd Annual Symposium on Computer Architecture*, pp. 191-202, May 1996.

### 第六章：内存系统
M. V. Wilkes, "Slave memories and dynamic storage allo-cation," *IEEE Transactions on Electronic Computers*, EC-14(2):270-271, 1965.

J. S. Liptay, "Structural aspects of the System/360 model 85, part II: The cache," *IBM Systems Journal*, 7(1): 15-21, 1968.

D. Kroft, "Lockup-free instruction fetch/prefetch cache organization," *Proceedings of the Eighth Symposium on Computer Architecture* pp. 81-87, May 1981.

J. R. Goodman, "Using cache memory to reduce processor-memory traffic," *Proceedings of the Tenth International Symposium on Computer Architecture*, Stockholm, Sweden, pp. 124-131, June 1983.

N. P. Jouppi, "Improving direct-mapped cache performance by the addition of a small fully-associative cache and prefetch buffers," *Proceedings of the 17th Annual Symposium on Computer Architecture, Computer Architecture News*, 18(2):364-373, 1990

T. Kilburn, D. B. G. Edwards, M. J. Lanigan, and F. H. Sumner, "One-level storage system," *IRE Transactions*, EC-11(2):223-235, 1962.

D. W. Clark and J. S. Emer, "Performance of the VAX-11/780 translation buffer: Simulation and measurement," *ACM Transactions on Computer Systems*, 3(1):31-62, 1985.

W.-H. Wang, J.-L. Baer, and H. M. Levy, "Organization and performance of a two-level virtual-real cache hierarchy," *Proceedings of the 16th Annual International Symposium on Computer Architecture*, Jerusalem, pp. 140-148, June 1989.

### 第七章：输入/输出：外部存储系统、网络与图形
M. Smotherman, "A sequencing-based taxonomy of 1/O systems and review of historical machines," *ACM Computer Architecture News* 17(5):5-15, Sept. 1989.

*外部存储系统*

C. Ruemmler and J. Wilkes, "An introduction to disk drive modeling," *IEEE Computer* 27(3): 17-28, 1994.

D. A. Patterson, G. Gibson, and R. H. Katz, "A case for redundant arrays of inexpensive disks (RAID)." *Proceedings of the ACM SIGMOD Conference*, Chicago, IL, June 1988.

*网络*

R. M. Metcalfe and D. R. Boggs, "Ethernet: Distributed packet switching for local computer networks." *Communications of the ACM*, 19(7):395-404.

L. M. Ni and P. K. McKinley, "A survey of wormhole routing techniques in direct networks," *IEEE Computer*, 26(2):62-76, 1993.

*图形*

K. Akeley, "Reality engine graphics," *SIGGRAPH '93 Proceedings*, pp. 109-116.

### 第八章：单指令多数据（SIMD: Single-Instruction Multiple Data）并行
M. J. Flynn, " Very high-speed computing systems," *Proceedings of the IEEE*, vol. 54, no. 12, Dec. 1966.

D. J. Kuck and R. A. Stokes, "The Burroughs scientific processor (BSP)," *IEEE Transactions on Computers*, C-31(5):363-376, 1982.

M. Gokhale, B. Holmes, and K. lobst, "Processing in mem-ory: The Terasys massively parallel PIM array," *IEEE Computer*, 28(4):23-31, 1995.

### 第九章：多处理器和多计算机
W. A. Wulf and S. P. Harbison, "Reflections in a pool of processors/An experience report on C.mmp/Hydra," *Proceedings of the National Computer Conference (AFIPS)*, June 1978.

L. Lamport, "How to make a multiprocessor computer that correctly executes multiprocess programs," *IEEE Transactions on Computers*, C-28(9):690-691, 1979.

L. M. Censier and P. Feautrier, "A new solution to coherence problems in multicache systems," *IEEE Transactions on Computers*, C-27(12):1112-1118, 1978.

D. Lenoski, J. Laudon, K. Gharachorloo, W.-D. Weber, A. Gupta, J. Hennessy, M. Horowitz, and M. Lam, "The Stanford Dash multiprocessor," *IEEE Computer*, 25(3):63-79, 1992.

E. Hagersten, A. Landin, and S. Haridi "DDM—A cache-only memory architecture," *IEEE Computer*, 25(9):44-54, 1992.

C. L. Seitz, "The cosmic cube," *Communications of the ACM*, pp. 22-33, Jan. 1985.

K. Li and P. Hudak, "Memory coherence in shared virtual memory systems," *ACM Transactions on Computer Systems*, 7(4):321-359, 1989.

### 第十章：当前实现与未来微处理器
D. Alpert and D. Avnon, "Architecture of the Pentium microprocessor," *IEEE Micro*, 13(3):11-21, 1993.

D. Papworth, "Tuning the Pentium Pro microarchitecture," *IEEE Micro*, 16(2):8-15, 1996.

M. Slater, "The microprocessor today," *IEEE Micro*, 16(6):32-44, 1996.

A. Yu, "The future of microprocessors," *IEEE Micro*, 16(6):46-53, 1996.
