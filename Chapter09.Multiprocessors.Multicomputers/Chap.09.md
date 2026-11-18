# 第9章：多处理器和多计算机


 * 一个处理器池的倒影(反思)/C.mmp/Hydra的一份精英报告 ---------------------- 561  
  *W. A. Wulf and S. P. Harbison*  
 * 如何制造一台能够正确执行多进程程序的多处理器计算机------ 574  
  *L. Lamport*  
 * 多缓存系统中一致性问题的新解决方案 -------------- 576  
  *L. M. Censier and P. Feautrier*  
 * 斯坦福 Dash 多处理器 --------------------------------------------- 583  
  *D. Lenoski, J. Laudon, K. Gharachorloo, W.-D. Weber, A. Gupta, J. Hennessy, M. Horowitz, and M. S. Lam*  
 * DDM-一种仅缓存存储体系结构 ------------------------------------------------- 600  
  *E. Hagersten, A. Landin, and S. Haridi  
 * 宇宙立方体------------------------------------------------------------------ 611  
  *C. L. Seitz*  
 * 在共享虚存系统中的存储一致性 -------------------------  623  
  *K. Li and P. Hudak*  

## 9.1 引言

&emsp;&emsp; 如今大多数计算机都使用单个微处理器作为中央处理器。这类系统——被称为“无处理器”——能够满足大多数用户的需求，因为它们的处理能力以每年约 50% 的复合增长率持续增长。然而，一些用户和应用程序仍然渴望更强大的处理能力。在线事务处理 (OLTP) 系统希望处理更多用户，天气预报员希望提高明日天气预报的准确性，虚拟现实系统则希望实现更精确的视觉效果。如果想要速度提升五倍，除了等待四年（1.54 = 5 倍）之外，还有哪些替代方案？

&emsp;&emsp; 一种提升速度的方法是尝试构建速度更快的处理器，例如使用更多并行逻辑门和/或采用更先进的技术。这正是 20 世纪 70 年代和 80 年代超级计算机所采用的方法。例如，Cray-1 [30] 在 1976 年就使用了 80 MHz 的时钟频率。而微处理器直到 20 世纪 90 年代初才达到这一时钟频率。

&emsp;&emsp; 然而，如今这种采用特殊处理器的方案在经济上不可行。微处理器通过将数千万（不久的将来可能是数亿）个晶体管与短导线集成在一起，形成复杂的流水线和集成存储系统，从而提供最快的计算引擎。这些设计需要经过精心调校，并采用昂贵的制造工艺来实现。微处理器的销量必须达到数百万台才能充分摊销前期成本。定制的微处理器出货量不足以摊销复杂流水线和集成存储系统的设计和测试成本。非微处理器虽然也可以采用特殊技术，但其导线更长——因此线路延迟也更长——并且同样面临设计和测试成本问题。

&emsp;&emsp; 因此，要想超越“非处理器”，唯一可行的方案是采用多个微处理器，并以某种方式将它们连接起来，使它们能够协同处理同一个问题。由于“非处理器”的存储系统和I/O系统长期分离，因此连接处理器的两种方案分别是通过存储系统和通过I/O系统也就不足为奇了。无论哪种情况，这些系统中的处理器都独立执行指令，因此是多指令多数据流 (MIMD) 系统，而非第 8 章所述的单指令多数据流 (SIMD) 系统。

&emsp;&emsp; 共享内存多处理器通过逻辑共享内存将多个处理器连接起来。硬件确保处理器对物理地址的存储操作对所有其他处理器可见。因此，可以使用常规的加载和存储操作进行通信。共享内存多处理器通常也具有逻辑共享的 I/O 系统。实现共享内存多处理器具有挑战性，因为良好的性能要求逻辑共享内存（和 I/O）在物理上是分散的。

&emsp;&emsp; 多个处理器也可以通过 I/O 系统连接起来。采用这种方法，每个处理器都可以位于一个配备网络接口 (NI) 的传统计算机节点中。 NI（网络接口）通常位于 I/O 总线上，可以是支持标准网络协议（例如 TCP/IP）的标准 NI（例如以太网 NI），用于将节点连接到局域网甚至互联网。我们不再深入探讨这种情况，而是建议读者参考网络方面的教材，例如 Peterson 和 Davie 的著作 [28]。

&emsp;&emsp; 我们将着重探讨通过自定义 NI 和/或自定义网络连接计算机的系统。这些系统（有时被称为多计算机系统）的 NI 和网络各不相同，从专用型（例如 Intel 系列多计算机系统）到接近局域网的系统（例如 Myricom Myrinet [6]）。与共享内存多处理器不同，多计算机系统不使用存储和加载操作进行通信。相反，硬件支持在节点之间传递消息的显式机制。实现高性能多计算机系统极具挑战性，因为在发送和接收每条消息时很难避免过大的软件开销。

&emsp;&emsp; 并行计算对计算机技术产生了重大影响。此外，我们预计它在未来将变得更加重要。传统上，并行计算被视为一种不惜一切代价加速高端应用程序的方法。如今，我们认识到它也可以具有成本效益[41]。如果一个多处理器系统在执行工作负载方面的速度提升超过了其成本溢价（相对于单处理器系统），那么相对于单处理器系统，多处理器系统就具有成本效益。在许多情况下，增加处理器的成本溢价很低，因为大部分成本都投入到了内存和I/O系统中。例如，1995年，一台配备1GB内存的八处理器Silicon Graphics Challenge XL的成本是同等单处理器系统的2.5倍[41]。因此，对于所有速度提升超过2.5倍的工作负载，八处理器XL都具有成本效益。

&emsp;&emsp; 接下来的章节将讨论并行计算机软件、共享内存多处理器和多计算机。关于并行软件和硬件的更多背景知识，可参阅 Almasi 和 Gottlieb [3] 以及 Culler、Singh 和 Gupta [11] 的著作。这两本书对本文提出的观点产生了显著影响。

## 9.2 并行计算机软件

&emsp;&emsp; 现代并行系统具有三个接口层级：用户编程模型、应用程序二进制接口 (ABI) 和硬件。编译器和库负责将应用程序映射到 ABI。ABI 通过操作系统服务和设备驱动程序在硬件上实现。为了设计并行硬件，我们必须了解这些接口以及在它们之间进行转换的软件。

### 9.2.1 编程模型

&emsp;&emsp; 对于非处理器，编程讨论有时会考虑不同编程语言的相对优劣，例如 Fortran、C、C++ 或 Java。这些语言实际上非常相似，对硬件的要求也类似。然而，在并行计算机编程中，关于 Fortran 与 C 语言孰优孰劣的争论远不如诸如数据共享是通过写入变量还是发送消息等更根本的问题重要。这些问题决定了所使用的编程模型。

&emsp;&emsp; 四种最流行的编程模型是顺序模型、数据并行模型、共享内存模型和消息传递模型。在顺序模型中，程序员编写常规的非处理器程序，并依赖系统软件（主要是编译器）来并行化其代码和数据。这种方法减轻了程序员并行化的负担，并且在某些情况下取得了成功，但尽管经过数十年的研究，它仍然不够稳健，无法普遍应用。

&emsp;&emsp; 在数据并行模型中，程序员编写的代码在控制流上是顺序的，但在数据操作上是并行的（例如，在高性能 Fortran 中）。

&emsp;&emsp; 假设以下所有变量都表示矩阵：

$ A = B + C $

$ E = A * D $

&emsp;&emsp; 在数据并行模型中，所有处理器似乎都会先执行矩阵加法中的多次加法运算，然后再开始执行矩阵乘法中的乘法和加法运算。在许多方面，数据并行编程是对单指令多数据流 (SIMD) 处理的抽象。它可能非常成功，但许多其他重要的程序在这种模式下运行不佳，或者难以表达。

&emsp;&emsp; 共享内存模型基于多个顺序线程通过共享变量共享数据。它提供的模型与多任务单处理器相同。该模型在中小型系统（少于 30 个处理器）中取得了巨大成功，例如在线事务处理和科学模拟等应用。共享内存编程的最大挑战在于如何实现充分的同步，以确保数据通信有效（例如，加载操作不会在写入数据的存储操作执行之前读取数据）。

&emsp;&emsp; 最后，消息传递模型允许使用传统的单处理器语言进行编程，并辅以显式的通信调用。在大多数情况下，通信由发送调用发起，该调用将数据从缓冲区复制到目标位置。应用程序通常使用接收调用来获取通信的数据，该调用将数据复制到指定的缓冲区。如果数据尚未就绪，接收调用可能会阻塞或返回未就绪标志。消息传递模型与网络计算中使用的模型非常相似。消息传递模型取得了许多成功，但它也使得正确实现复杂的、基于指针的数据结构变得困难。

### 9.2.2 应用程序二进制接口 (ABI)

&emsp;&emsp; 在并行计算的大部分历史中，编程模型和硬件之间一直没有分离。相反，机器的特征在于它们支持的编程模型。数据并行或SIMD机器包括Illinois ILLIAC IV和Thinking Machines CM-1。消息传递机器包括 Caltech Cosmic Cube、Intel Hypercube 和 Intel Paragon。共享内存机器包括 CMU C.mmp、Sequent Balance 和 SGI 2000。

&emsp;&emsp; 今天我们了解到编程模型和硬件是不同的。任何硬件都可以支持任何编程模型。现代系统使用编译器和库将应用程序映射到 ABI。然后，他们使用操作系统服务和设备驱动程序在硬件上实现 ABI。接下来的挑战是确定运行良好的 ABI 和硬件机制。

&emsp;&emsp; 当今的并行系统已发展为两类 ABI：共享内存和消息传递。共享内存 ABI 为应用程序提供了类似于多任务非处理器的接口。编译器和库将用用户编程模型编写的应用程序映射到共享内存 ABI。将顺序编程和数据并行编程模型映射到共享内存 ABI 需要广泛的编译器支持。支持共享内存很简单。实现消息传递只需要一个在共享内存中实现发送和接收调用的库。

&emsp;&emsp; *消息传送ABI* 为应用程序提供了类似于网络通信的界面。节点具有独立的内存和独立的I/O空间，但可以通过消息交换数据。将用户编程模型映射到消息传递 ABI 具有挑战性。在消息传递 ABI 上实现顺序和数据并行编程模型需要比在共享内存 ABI 上更好的编译器支持，因为不必要的消息的成本通常高于不必要的内存引用的成本。当需要良好的性能时，在消息传递 ABI 上实现共享内存编程模型也很困难。然而，支持消息传递 ABI 上的消息传递程序只需要一个简单的库。最流行的科学编程库是消息传递接口 (MPI) [16, 35]。

### 9.2.3 硬件

&emsp;&emsp; 最后，必须在两类硬件平台（多处理器和多计算机）上实现两种 ABI：共享内存和消息传递。 
  - *共享内存多处理器上的共享内存ABI*。最重大的挑战是开发一个在许多处理器上运行良好的操作系统。 
  - *共享内存多处理器上的消息传递ABI*。可以使用标准操作系统机制来完成通信，前提是它们性能足够好。 
  - *多计算机上的共享内存ABI*。这很难。稍后讨论的 Li 和 Hudak [26] 给出了共享内存的一些答案，但共享 I/O 也必须实现。 
  - *在多计算机上发送消息ABI*。这可以通过具有标准网络的标准操作系统来实现。面临的挑战是获得良好的消息传递性能，尤其是在每条消息都涉及操作系统的情况下。

&emsp;&emsp; 编程模型、ABI 和硬件之间的关系可能会令人困惑，但区分这些区别可能很重要。例如，结果表明，(1) 多处理器上的共享内存 ABI 上的消息传递编程可以比 (2) 多处理器上的共享内存 ABI 上的共享内存编程或 (3) 多计算机上的消息传递 ABI 上的消息传递编程带来更高的性能。

&emsp;&emsp; 现在进行一个测验：(1) 多计算机上的消息传递 ABI 上的共享内存程序和 (2) 多计算机上的共享内存 ABI 上的共享内存程序有什么区别？答案：在（1）中，编译器或库将对共享变量的访问转换为消息（例如，Berkeley Split-C[10]）。在 (2) 中，应用程序实际上使用加载和存储指令，并且系统使事情正常工作（例如，Li 和 Hudak 的 Ivy [26]）。

## 9.3 共享内存多处理器

&emsp;&emsp; 本节探讨共享内存多处理器的发展历程。在这些机器中，处理器可以使用常规的内存访问指令（例如，加载和存储指令）访问所有内存，并且所有处理器都可以使用相同的地址访问同一内存位置。

&emsp;&emsp; 大多数共享内存多处理器都允许处理器拥有缓存。缓存的工作原理是复制数据，并将副本与数据的原始地址关联起来。确保处理器（和设备）获取并更新数据的最新副本是缓存一致性问题，通常通过缓存一致性协议（例如，第 6 章中描述的 Goodman 的一次写入协议）来解决。使用缓存一致性的多处理器通常被标记为 CC，而未使用缓存一致性的多处理器则被标记为 NCC。

&emsp;&emsp; 多处理器之间的区别还在于所有内存访问是否都遇到相似的延迟（统一内存访问或 UMA）或并非如此（非 UMA 或 NUMA）。这导致多处理器可分为四类：NCC-UMA、NCC-NUMA、CC-UMA 和 CC-NUMA。[^1] CC-UMA 也称为对称多处理器 (SMP)。

[^1]：NCC-UMA 和 NCC-NUMA 机器通常分别被称为 UMA 和 NUMA。我们添加前缀 NCC 是为了避免歧义。

### 9.3.1 Wulf 和 Harbison 的《处理器池中的反思/C.mmp/Hydra 经验报告》[42]

&emsp;&emsp; Wulf 和 Harbison 总结了 20 世纪 70 年代一项关于多处理器硬件和操作系统的开创性研究项目。该项目于 1972 年在卡内基梅隆大学启动，开发了 C.mmp 硬件和 Hydra 操作系统。 C.mmp 使用交叉开关（而非缓存）实现了所有 16 个 DEC PDP-11 处理器对 16 个交错内存模块的统一延迟访问。它率先采用了使用现成组件的 NCC-UMA 架构。Hydra 是一个对称操作系统（没有主从关系），它将策略与内核机制分离，并使用了功能。

&emsp;&emsp; Wulf 和 Harbison 的论文也值得一提，它是我们见过的最全面、最平衡的研究项目回顾之一。如果所有研究项目都能像 C.mmp 一样坦诚地分享其技术和非技术方面的成功与失败，我们就能彼此学习到更多。希望深入了解这一时期的读者还可以阅读 C.mmp 的后续项目 CMU CM* [36]。

### 9.3.2 Lamport 的《如何构建一台能够正确执行多进程程序的多处理器计算机》[22]

&emsp;&emsp; 随着体系结构设计师开始考虑对写缓冲区和缓存等共享内存系统进行性能优化，明确定义正确的共享内存应该如何运作变得尤为重要。在单处理器中，对内存的加载操作应该返回上次存储到同一地址的值。在多任务单处理器中，加载操作也应该返回上次存储到同一地址的值，但此时该存储操作可能来自加载操作所在的线程，也可能来自自加载操作所在的线程上次存储该地址以来被复用到该处理器上的另一个线程。

Lamport 正是利用这种推理来定义*顺序一致性*（SC）。通俗地说，如果一个多处理器始终表现得像一个多任务单处理器，那么它就实现了顺序一致性。Lamport 用一个所有内存操作的全序关系来形式化这一概念，该全序关系遵循每个处理器上的程序顺序。 SC 的实现必须对程序员隐藏性能优化机制，例如写缓冲区和缓存。

为了提升性能，人们提出并部署了其他内存一致性模型。一些模型将先进先出（FIFO）写缓冲区暴露给程序员：处理器一致性、SPARC TSO 和 Intel IA-32。另一些模型则允许某些内存操作完全乱序执行：弱一致性、释放一致性、DEC/Compaq Alpha、IBM PowerPC 和 Sun RMO。Adve 和 Gharachorloo [1] 提供了关于替代模型的最新教程，而 Hill [19] 则认为推测执行应该促使共享内存多处理器回归顺序一致性。

### 9.3.3 窥探缓存一致性

Goodman 的论文“利用缓存减少处理器-内存流量”[15] 出现在第 6 章，并在该章中有更详细的描述。本章重点在于 Goodman 对“一次写入”协议的定义，这是第一个窥探缓存一致性协议。在首次写入数据块时，“一次写入”协议执行直写操作，该操作也会使其他缓存副本失效。在后续写入操作中，则使用回写操作。“一次写入”协议催生了一系列替代的窥探协议，这些协议后来被整合到 MOESI 框架 [37] 中。

更重要的是，窥探协议催生了最成功的商用多处理器类型：CC-UMA 或对称多处理器 (SMP)。早期商用 SMP 的代表是 Sequent Balance [39]。Sun UltraEnterprise 5000 [34] 是目前最先进的 SMP 的代表。其总线支持来自多达 30 个 CPU 的 112 个并发事务，并在事务开始后立即更新一致性状态，且与数据传输顺序无关。

### 9.3.4 Censier 和 Feautrier 的“多缓存系统中一致性问题的新解决方案”[8]

Censier 和 Feautrier 提出了第一个适用于大规模处理器的目录缓存一致性协议方案。该论文早于窥探技术的发明，但直到现在才在商业系统中变得重要。内存中的每个块都有一个目录项，用于提供有关该块缓存状态的信息。无论块被缓存到何处，目录项的位置都是已知的。Censier 和 Feautrier 的目录项包含一个位，用于标识哪个处理器缓存了该块。该方案后来被 Agarwal 等人 [2] 归类为 Dir,NB。在每个缓存中，两个状态位用于标识块是已修改（因此是独占的）、有效（但未修改且可能被共享）还是无效。当处理器读取无效数据块或写入未修改的数据块时，会向目录发送请求，目录可能会发送消息以获取该数据块或使即将失效的副本失效。

在 Censier 和 Feautrier 之前，Tang [38] 开发了一种更适合少量处理器的一致性协议，因为它在内存控制器处为所有处理器复制缓存标签。

### 9.3.5 Lenoski 等人的“斯坦福 DASH 多处理器”[25]

在 Censier 和 Feautrier 提出目录方案十年后，斯坦福大学在 DASH 共享内存多处理器原型中实现了该理念的改进。斯坦福 DASH 使用两个二维网状网络连接了多达 16 个 SGI 四路 SMP。它采用 CC-NUMA 架构和分布式目录协议实现了共享内存。该项目因其在一致性协议、内存一致性模型、基准测试等方面的诸多研究成果而备受瞩目。同样重要的是，DASH 将所有内容整合在一起，以探索竞争条件、死锁和其他实现问题。这项探索为后续的商业产品铺平了道路，例如 SGI Origin 2000 [23]。

### 9.3.6 Hagersten、Landin 和 Haridi 的“DDM——仅缓存内存架构”[17]

Hagersten 等人指出，CC-NUMA机器需要消耗大量内存来保存来自更远内存模块的本地缓存数据副本。为了减少这种浪费，研究人员提出将所有内存都转化为缓存，这样在一个节点上使用的数据就不必存在于未使用的远程节点上。这种架构被称为纯缓存内存架构（COMA）。实现 COMA 的关键挑战包括：找到没有永久存储位置的数据，以及在系统其他位置不存在数据时，如何从缓存中替换数据。这些想法最初是为 Kendall Square Research 公司的商用机器 KSR-1 开发的 [31]。迄今为止，还没有商业上成功的 COMA 机器，但研究人员仍在探索如何改进这一理念，使其更易于实现。

### 9.3.7 多处理器的现状与未来

共享内存多处理器 (SMP) 目前已广泛应用于服务器和大型计算机。其中大多数是对称多处理器 (CC-UMA)，它们使用总线。少数大型机器，例如 Silicon Graphics Origin 2000，采用 CC-NUMA 设计。Cray T3E 是一款 NCC-NUMA [32]。我们目前尚未发现任何 NCC-UMA。

基于成本效益方面的考虑，我们预计未来会有更多桌面计算机采用 SMP（CC-UMA）。SMP 将继续在低端服务器领域蓬勃发展，而基于目录的 CC-NUMA 将在高端领域得到更广泛的应用。SMP 和 CC-NUMA 之间的具体差异将取决于设计这些系统的工程师的创造力。我们预计 NCC-NUMA 和 NCC-UMA 将会逐渐被淘汰，因为一致性缓存对于获得通用用户和系统软件的良好性能至关重要。

## 9.4 多计算

本节重点讨论不全局共享内存的系统。相反，节点包含一个或多个处理器、本地共享内存、可选的 1/0 设备以及互连网络接口。

处理器使用常规的内存访问指令来获取节点内的数据，但使用其他机制来获取远程数据。极端情况下，互联网上的主机集合符合上述定义。然而，我们将重点讨论耦合性更强的系统。这些系统有时被称为多计算机或 NORMAS（禁止远程内存访问）。

### 9.4.1 Seitz 的“宇宙立方体”[33]

Seitz 描述了加州理工学院的宇宙立方体。宇宙立方体开创了多计算机硬件架构和消息传递编程模型。它包含 64 个节点，这些节点通过超立方体网络连接。每个节点包含一个 Intel 8086/8087 处理器和 128 KB 内存。它采用消息传递编程。通过消息传递，每个节点的程序都增加了显式的 send() 和 receive() 调用，以执行节点间通信。

### 9.4.2 多计算机后续发展

加州理工学院的 Cosmic Cube 催生了一系列英特尔多计算机，包括 iPSC/1、iPSC/2 和 Paragon，并影响了所有其他多计算机供应商。这些机器被科研实验室和大学的科学家使用，但并未获得广泛的商业成功。问题之一是消息传递的开销很大（> 1 毫秒）。第二个问题是这些机器仅针对消息传递，而这种编程模型对于许多应用来说过于局限。

许多研究致力于降低消息传递的开销。这里我们列举四种。Thinking Machines CM-5 [24] 是一款商用多计算机，它将网络接口 (NI) 从 I/O 总线移到了内存总线。此外，它还将 NI 映射到用户空间，以便用户级软件可以使用非缓存的加载和存储操作来发送和接收消息。保护机制仍然通过可分区网络和特殊的上下文切换机制来维持。一个特殊的网络支持许多关联运算的快速全局归约（例如，每个节点贡献一个数字的整数之和）。

伯克利活动消息 [40] 是一种纯软件实现的方案，旨在将消息抽象层级降低到接近硬件实际可实现的程度。活动消息包含一个处理程序地址和零个或多个参数。到达目标地址的消息会生成一个线程，该线程从处理程序地址开始执行。没有显式的 `receive()` 调用。

MIT J-Machine [27] 探索了连接多个小型节点（例如 1024 个）的可能性。每个节点包含 1 MB 内存和一个定制的“消息驱动处理器”。该处理器对通信（消息发送指令和硬件接收/分发队列）、同步（通过标签）和命名（包含加载和查询关联表的指令）的定制支持尤其引人注目。

多计算机发展的最后一个方向是更有效地利用标准硬件和软件。IBM SP/2 使用运行 IBM 标准 Unix 的工作站主板，但它添加了定制的 NI 和定制网络，并将所有组件封装在一个定制的机箱中。其他系统则使用彼此靠近放置并通过定制局域网（例如 Myricom Myrinet）连接的传统工作站。最后，还可以运行像 Oak Ridge Parallel Virtual Machine (PVM) [13] 这样的系统，以利用组织内所有桌面电脑中闲置的工作站。

### 9.4.3 Li 和 Hudak 的《共享虚拟内存系统中的内存一致性》[26]

Li 和 Hudak 并没有提出一种新的多计算机系统。相反，他们提出了 Ivy——第一个在多计算机（实际上是工作站网络）上实现共享内存 ABI 的系统。这种系统——现在被称为*软件分布式共享内存*系统[^2]——允许共享内存应用程序二进制文件发出加载和存储操作，并透明地确保共享内存在整个系统中行为正确（例如，顺序一致性）。关键在于使用标准的虚拟内存页面保护机制（有效且可写）来模拟缓存状态。当加载或存储到不合适的页面时，硬件会生成一个缺页错误，该错误会被转发给 Ivy，以便 Ivy 可以发送消息（遵循目录协议）来获取数据并恢复应用程序。

[^2]：像 SGI Origin 2000 这样的机器被称为硬件分布式共享内存机器。有些人用 DSM 来指代软件分布式和硬件分布式共享内存机器，而另一些人则将 DSM 仅用于软件分布式共享内存机器。

后续的大量工作 [4, 29] 旨在改善 Ivy 的两个关键问题：(1) 由于维护整个页面的一致性而导致的伪共享；(2) 松耦合机器之间较长的消息延迟。一些解决方案通过在缓存块上维护一致性，但使用虚拟内存硬件命名本地缓存的远程数据，模糊了软件分布式共享内存和 COMA 之间的区别 [18, 20]。与 COMA 一样，DSM 的理念目前尚未取得任何实质性的商业成功。

### 9.4.4 多计算机的现状与未来

如今，最成功的多计算机的节点是（或类似于）工作站。 IBM SP/2 是最成功的产品。更广泛部署的是数百个采用“工作站网络”[5] 的站点。这些并非最优计算系统，但由于工作站已部署在人们的办公桌上，因此采用它们的边际成本几乎为零。然而，我们认为，对于大规模计算而言，多计算机的重要性将不如多处理器，因为消息开销会消耗过多的性能。

但是，多计算机在高可用性计算领域现在和将来都将蓬勃发展。可用性是指在给定时间，系统有足够的资源能够执行给定任务的概率。高可用性对于 Web 服务器至关重要，对于数据库更是不可或缺。多计算机非常适合高可用性，因为当一个节点崩溃时，隔离多计算机节点相对容易。这使得一个 n 节点系统能够在另一个节点重启的同时，继续以 (n - 1)/n 的吞吐量运行。

高可用性多计算机将持续发展。如今，大多数多计算机系统都通过标准局域网（例如 100 Mbps 以太网）连接，但也有一些系统使用定制局域网，例如 Myricom Myrinet。有人认为，定制局域网将发展成为一种新型网络——系统区域网络（SAN），其带宽和延迟性能优于局域网 [21]。

最后，多计算机和多处理器之间的界限可能会变得模糊。推动这一趋势的一个方向是，努力使多计算机能够在无需操作系统参与每条消息的情况下共享信息。诸如 DEC 内存通道 [14] 和 Compaq/Intel/Microsoft 的虚拟接口架构 [12] 等方案，可以使多计算机的通信带宽和延迟更接近多处理器。或者，如果系统的部分组件即使在其他组件崩溃的情况下也能保持可用，那么多处理器也可以更像多计算机。Stanford HIVE [9] 和 DISCO [7] 等技术正是朝着这个方向迈出的步伐。

## 9.5 参考文献


[1] S. V. Adve and K. Gharachorloo, "Shared memory consistency models: A tutorial," IEEE Computer, 29(12):66-76, Dec. 1996.  
[2] A. Agarwal, R. Simoni, M. Horowitz, and J.
Hennessy, "An evaluation of directory schemes for cache coherence," Proceedings of the 15th Annual International Symposium on Computer Architecture, pp. 280-289, 1988.  
[3] G. S. Almasi and A. Gottlieb, Highly Parallel Computing. Menlo Park, CA: Benjamin/Cummings, 1994.  
[4] C. Amza, A. L. Cox, S. Dwarkadas, P. Keleher, H. Lu, R. Rajamony, W. Yu, and W. Zwanepoel, "TreadMarks: Shared memory computing on networks of workstations," IEEE Computer, 29(2): 18-28, 1996.  
[5] T. E. Anderson, D. E. Culler, D. A. Patterson, and the NOW team, "A case for NOW (networks of workstations)," IEEE Micro, 15(1):54 64, 1995.  
[6] N. J. Boden, D. Cohen, R. E. Felderman, A. E. Kulawik, C. L. Seitz, J. N. Seizovic, and W.-K. Su, "Myrinet: A gigabit-per-second local area network," IEEE Micro, 15(1):29-36, 1995.  
[7] E. Bugnion, S. Devine, K. Govil, and M. Rosenblum, "Disco: Running commodity operating systems on scalable multiprocessors," ACM Transactions on Computer Systems, 15(4):412-447, 1997.  
[8] L. M. Censier and P. Feautrier, "A new solution to coherence problems in multicache systems," IEEE Transactions on Computers, C-27(12):1112-1118, 1978.  
[9] J. Chapin, M. Rosenblum, S. Devine, T. Lahiri, D. Teeodosiu, and A. Gupta "Hive: Fault containment for shared-memory multiprocessors," Proceedings of the 15th ACM Symposium on Operating System Principles (SOSP), pp. 12-25, Dec. 1995.  
[10] D. E. Culler, A. Dusseau, S. C. Goldstein, A. Krishnamurthy, S. Lumetta, T. von Eicken, and K. Yelick, "Parallel programming in Split-C," Proceedings of Supercomputing '93, pp. 262-273, Nov. 1993.  
[11] D. Culler, J. P. Singh, and A. Gupta, Parallel Computer Architecture: A Hardware/Software Approach. San Francisco, CA: Morgan Kaufmann, 1998.  
[12] D. Dunning, G. Regnier, G. McAlpine, D. Cameron, B. Shubert, F. Berry, A. M. Merritt, E. Gronke, and C. Dodd, "The virtual interface architecture," IEEE Micro, 18(2):66-76, 1998.  
[13] A. Geist, A. Beguelin, J. Dongarra, W. Jiang, R. Manchek, and V. Sunderam, in PVM: A users' guide and tutorial for networked parallel computing. Cambridge, MA: MIT Press, 1994. Available electronically at <http://www.netlib.org/pvm3/book/pvm-book.ps>  
[14] R. B. Gillett, "Memory channel network for PCI," IEEE Micro, 16(1):12-18, 1996.  
[15] J. R. Goodman, "Using cache memory to reduce processor-memory traffic," Proceedings of the Tenth International Symposium on Computer Architecture, pp. 124-131, June 1983.  
[16] W. Gropp, S. Huss-Lederman, A. Lumsdaine,
E. Lusk, B. Nitzberg, W. Saphir, and M. Snir, MPI-The Complete Reference: Vol. 2, The MPI-2 Extensions. Cambridge, MA: MIT Press, 1998.  
[17] E. Hagersten, A. Landin, and S. Haridi, "DDM-A cache-only memory architecture," IEEE Computer, 25(9):44-54, 1992.  
[18] E. Hagersten, A. Saulsbury, and A. Landin, "Simple COMA node implementations," Proceedings of the 27th Hawaii International Conference on System Sciences, Jan. 1994.  
[19] M. D. Hill, "Multiprocessors should support simple memory consistency models," IEEE Computer, 31(8):28-34, 1998.  
[20] M. D. Hill, J. R. Larus, and D. A. Wood, "Tempest:
A substrate for portable parallel programs," Proceedings of COMPCON '95, pp. 327-332, San Francisco, CA, Mar. 1995.  
[21] R. W. Horst, 'TNet: A reliable system area network," IEEE Micro, 15(1):37-45, 1995.  
[22] L. Lamport, "How to make a multiprocessor computer that correctly executes multiprocess programs," IEEE Transactions on Computers, C-28(9):690-691, 1979.  
[23] J. Laudon and D. Lenoski, "The SGI origin: A CNUMA highly scalable server," Proceedings of the 24th Annual International Symposium on Computer Architecture, pp. 241-251, June 1997.  
[24] C. E. Leiserson, Z. S. Abuhamdeh, D. C. Douglas, C. R. Feynman, M. N. Ganmukhi, J. V. Hill, W. D. Hillis, B. C. Kuszmaul, M. A. St. Pierre, D. S. Wells, M. C. Wong, S.-W. Yang, and R. Zak, "The network architecture of the connection machine CM-5," Proceedings of the Fifth ACM Symposium on Parallel Algorithms and Architectures (SPAA), July 1993.  
[25] D. Lenoski, J. Laudon, K. Gharachorloo, W.-D. Weber, A. Gupta, J. Hennessy, M. Horowitz, and M. Lam, "The Stanford DASH multiprocessor," IEEE Computer, 25(3):63-79, 1992.  
[26] K. Li and P. Hudak, "Memory coherence in shared virtual memory systems," ACM Transactions on Computer Systems, 7(4):321-359, 1989.  
[27] M. D. Noakes, D. A. Wallach, and W. J. Dally, "The J-machine multicomputer: An architectural evaluation," Proceedings of the 20th Annual International Symposium on Computer Architecture, pp. 224-235, May 1993.  
[28] L. L. Peterson and B. S. Davie, Computer Networks: A Systems Approach. San Francisco, CA: Morgan Kaufmann, 1996.  
[29] J. Protic, M. Tomasevic, and V. Milutinovic, "Distributed shared memory: Concepts and systems," IEEE Parallel & Distributed Technology, Systems, & Applications, 4(2):63-79, 1996.  
[30] R. M. Russell, 'The Cray-1 computer system" Communications of the ACM, 21(1):63-72, 1978.  
[31] R. H. Saavedra, R. S. Gaines, and M. J. Carlton, "Micro benchmark analysis of the KSR1," Proceedings of Supercomputing '93, pp. 202-213, Nov. 1993.  
[32] S. L. Scott, "Synchronization and communication in the 3E multiprocessor," Proceedings of the 7th International Conference on Architectural Support for Programming Languages and Operating Systems, pp. 26-36, Oct. 1996.  
[33] C. L. Seitz, "The cosmic cube," Communications of the ACM, pp. 22-33, Jan. 1985.  
[34] A. Singhal, D. Broniarczyk, F. Cerauskis, J. Price, L. Yaun, C. Cheng, D. Doblar, S. Fosth, N. Agarwal, K. Harvery, E. Hagersten, and B. Liencres, "Gigaplane: A high performance bus of large SMPs," Proceedings of the IEEE Hot Interconnects, pp. 41-52, Aug. 1996.  
[35] M. Snir, S. Otto, S. Huss-Lederman, D. Walker, and J. Dongarra, MPI-The Complete Reference: Vol. 1, The MPI Core. Cambridge, MA: MIT Press, 1998.  
[36] R. J. Swan, S. H. Fuller, and D. P. Siewiorek, "Cm* — A modular, multi-microprocessor," Proceedings of the AFIPS National Computer Conference, pp. 637-644, 1977.  
[37] P. Sweazey and A. J. Smith, "A class of compatible cache consistency protocols and their support by the IBEE Futurebus," Proceedings of the 13th Annual International Symposium on Computer Architecture, pp. 414-423, June 1986.  
[38] C. K. Tang, "Cache system design in the tightly compled multiprocessor system," Proceedings of the AFIPS National Computing Conference, pp. 749-753, June 1976.  
[39] S. Thakkar, P. Gifford, and G. Fielland, "Balance: A shared memory multiprocessor system," Proceedings of the 2nd International Conference on Supercomputing, pp. 93-101, 1987.  
[40] T. von Eicken, D. E. Culler, S. C. Goldstein, and K. E. Schauser, "Active messages: A mechanism for integrating communication and computation," Proceedings of the 19th Annual International Symposium on Computer Architecture, pp. 256-266, May 1992.  
[41] D. A. Wood and M. D. Hill, "Cost-effective parallel computing," IEEE Computer, 28(2):69-72, 1995.  
[42] W. A. Wulf and S. P. Harbison, "Reflections in a pool of processors/An experience report on C.m mp/Hydra," Proceedings of the National Computer Conference (AFIPS). pp. 939-951, June 1978.  
