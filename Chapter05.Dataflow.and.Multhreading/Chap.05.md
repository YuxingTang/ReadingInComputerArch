# 第5章 数据流和多线程

 * 基本数据流处理器的初步体系结构 ----------------------------------315  
  *J. B. Dennis and D. P. Misunas*
 * 在MIT Tagged-Token数据流体系结构上执行程序 ---------------------- 323  
  *Arvind and R. S. Nikhil*
 * HEP多处理器计算机系统的体系结构和应用  ----------------- 342  
  *B. J. Smith*
 * 挖掘选择：在一个可实现的同时多线程处理器上取指和发射 ---------------------- 350  
  *D. M. Tullsen, S. J. Eggers, J. S. Emer, H. M. Levy, J. L. Lo, and R. L. Stamm*

## 5.1 数据流计算机

如今大多数计算机都基于顺序执行模型，其中程序指令按照隐含的（顺序）顺序执行。这种模型与早期处理器的实现方式非常相似，当时程序指令是逐条处理的。顺序执行模型也催生了具有顺序执行语义的指令集和编程语言。如今，几乎所有程序都是用具有隐含执行顺序的编程语言编写并编译成指令集的。

为了最大限度地提高并行性，必须最大限度地减少顺序约束。理想情况下，指令的执行应该只受依赖关系的约束，而不受任何其他顺序约束的约束。因此，为了在顺序执行模型中实现高性能，需要从串行规范中发现并提取并行性，并在并行硬件上执行这些操作。数据流是一种旨在克服传统顺序计算模型性能限制的计算模型。传统（顺序）模型的诸多局限性在于：难以发现和提取因串行程序语义而隐藏的并行性，难以在并行硬件上利用这些并行性，也难以维护隐含的程序顺序。

数据流模型的核心思想是构建一个计算引擎，除了计算中存在的数据（和控制）依赖关系外，对操作执行没有任何顺序约束。计算过程被表示为一个数据流图，该图捕捉了操作之间的数据依赖关系。控制依赖关系的处理方式与数据依赖关系相同（通过使用 SWITCH 运算符，利用布尔条件来控制数据值）。由于不对程序执行施加“全序”，并行性的机会不会被人为地限制。在没有全序的情况下，需要其他规则来确定操作的执行顺序。数据流模型使用数据驱动（或依赖驱动）的规则来控制操作的执行。

数据流程序由指令块或代码块组成，其执行受数据流“触发”规则的约束。代码块的动态状态（类似于栈帧）存储在令牌存储器中。当一个操作执行时，它会“唤醒”令牌存储器中依赖于它的其他操作；当操作的操作数准备就绪时，该操作即可被“触发”。不同类型的数据流计算机使用不同的方法来唤醒操作。Arvind 和 Nikhil 在论文中描述的经典标记令牌数据流架构 (TTDA) 使用关联令牌存储器 [3]。当一个操作执行时，它会创建一个或两个结果令牌，每个令牌都带有一个标签，该标签指示需要该令牌的指令实例。系统会（关联地）搜索令牌存储器，以查看是否存在具有相同标签的另一个令牌。如果找到匹配项，则新指令即可执行。否则，该令牌将保留在令牌存储器中。由于令牌存储可能非常庞大，关联搜索通常通过哈希算法来实现[9,14]。最近提出的显式令牌存储（ETS）数据流机通过将代码块的动态实例映射到线性（可寻址）内存，消除了在令牌存储中进行关联搜索的需要[13]；令牌可以直接转发到已知内存地址的指令。

尽管数据流执行模型消除了人为的排序约束，但传统编程语言仍然存在排序约束，这些约束可能会在翻译过程中被保留下来，就像（传统）程序被编译成数据流可执行文件一样。为了克服这个问题，数据流研究人员提出了诸如Id[2]之类的数据流语言，这些语言没有人为的排序约束。

数据流计算系统（包括语言、编译器、指令集和执行硬件）能够充分展现和利用并行性。然而，并行性的过度存在未必是件好事：必须有相应的机制来处理这种并行性[4]。这些机制包括用于处理所有待执行操作的大型令牌存储、管理令牌存储的机制，以及调度（成百上千个）准备就绪的操作的机制，以便它们能够在有限的可用硬件上执行。在许多情况下，限制并行性实际上可以带来更高效的解决方案！

## 5.2 多线程计算机

多线程计算机是指能够执行来自多个执行线程的指令的计算机。然而，“多线程”一词在计算机科学领域，尤其是在计算机体系结构领域，含义非常广泛。线程是一段代码及其状态（寄存器和内存）；线程中的指令在执行过程中会修改该状态。在最常见的用法中，“多线程”指的是多个不同的程序（或进程）。然而，计算机体系结构师，尤其是处理器体系结构师，很少关注如何提高处理包含多个程序的工作负载时的整体吞吐量。他们更关注的是如何缩短单个程序的执行时间。在这种情况下，“多线程”指的是单个程序的不同（通常是独立的）部分；例如，循环的不同迭代。无论哪种情况，多线程处理器也可以被视为实现并行处理器的另一种方式（尽管规模较小）。

为了支持多线程，多线程处理器必须提供硬件资源来实现线程上下文，也就是每个线程的状态。线程的状态通常存在于两个命名空间中：寄存器和内存。多线程处理器通常为每个线程提供一个单独的寄存器文件。然而，（物理）内存是在不同线程之间共享的，就像在运行多程序工作负载的非多线程处理器中一样。

多线程最常见的用途是提高资源利用率。例如，当处理器在等待长延迟内存操作的结果时处于空闲状态，如果处理器将执行切换到另一个线程，并在原本空闲的周期执行该线程的指令，则可以提高处理器利用率（以及整体处理吞吐量）。通过在原本空闲的周期执行工作，处理器能够容忍较长的延迟。因此，许多人也将多线程视为容忍长延迟的一种手段。然而，只有当与长延迟操作重叠的操作来自同一程序时，容忍延迟对单个程序执行时间的益处才会显现；也就是说，多个线程必须是同一程序的线程。

多线程会带来额外的开销，因为需要提供额外的硬件来处理来自多个线程的状态。此外，它还会带来与线程切换相关的性能开销，这些开销会影响单线程的性能。首先，切换到不同的线程会产生开销，具体取决于不同线程的寄存器上下文的实现方式。可以通过增加硬件来降低这种开销的影响，从而实现线程间无时间损失的切换。其次，线程一旦被切换出去，可能不会在其长延迟操作完成后立即重启，这会导致线程的执行时间增加（相比之下，如果线程没有被切换出去，并且能够在长延迟操作完成后立即恢复执行，则不会出现这种情况）。几十年来，线程切换与单线程性能之间的权衡一直是多线程处理器设计的核心问题，人们提出了多种不同的切换策略。

一种常用的线程切换策略是在每个周期切换到不同的线程。这种策略实现起来相对简单。此外，如果线程数与流水线中的阶段数相同，则流水线中每个线程都只执行一条指令。这避免了使用专门的硬件来解决单线程指令处理重叠问题（例如，依赖性检查逻辑、分支预测硬件等）。然而，这种方法的缺点是牺牲了单线程的性能。


4,014 / 5,000
另一种策略是持续处理单个线程，直到遇到长延迟操作（例如缓存未命中）时切换到新线程[1]。这种策略对单线程性能的降低程度小于之前的方案（尽管在长延迟操作完成后重启线程的问题仍然存在），但更为复杂。此外，还有其他几种线程切换策略的提案。

20 世纪 90 年代之前研究的大多数多线程策略都针对每个时钟周期只能执行单个操作的处理器。对于这些处理器而言，只有在经过一个时钟周期（或多个时钟周期）后切换线程才有意义，因为一旦线程被选中，就至少应该执行一个操作，而这至少需要一个时钟周期。20 世纪 80 年代末和 90 年代多发射长指令字 (LIW) 和超标量处理器的出现促使研究人员重新思考线程切换的概念。多发射处理器允许在每个时钟周期内启动多个操作，但并非所有发射槽都能被给定线程在给定时钟周期内使用。在这种情况下，线程切换的粒度可以小于一个时钟周期（即在一个时钟周期内）：主线程未使用的发射槽可以被其他线程用于执行操作。这可能带来双赢：单个线程可以继续执行原本不会被使用的发射槽。当然，这假设硬件和软件的复杂性不会增加，并且不同的线程不会干扰机器的其他资源（例如缓存、内存系统和互连）。在单个时钟周期内执行多个线程的操作称为*同步多线程*。

由于同步多线程策略允许在不牺牲单线程性能的前提下实现多线程，并且半导体技术也提供了足够的硬件资源，可以在单个芯片上构建（寄存器）多个线程的上下文，计算机架构师正在考虑即使在通用处理器中也支持多线程。

读者可能会问：数据流和多线程似乎是截然不同的概念，为什么我们要把它们放在同一章中讨论呢？数据流和多线程的确是不同的概念。然而，出于我们将在下文讨论的原因，许多多线程研究都是在数据流的背景下进行的。

数据流的目标是展现和利用并行性，而这需要并行硬件。并行硬件会引入一些非并行硬件中可能不存在的延迟，例如遍历互连结构以及分配内存等资源所产生的延迟。在数据驱动执行模型中，当一个操作的前导操作向其传递值时，该操作才会被唤醒。并行硬件引入的延迟会加剧操作间的通信延迟，因此我们必须通过与其他（独立的）计算重叠来容忍这种延迟。此外，数据流程序的代码块可以被视为一个“线程”。[^1] 由于数据流程序的动态执行会导致多个这样的“线程”同时处于活动状态，因此设计能够处理多个此类线程的机器是自然而然的。

[^1]：如果我们根据机器支持的线程长度在多线程谱系上对其进行表征，那么纯粹的经典数据流（例如，MIT TDA）的线程长度为一个操作。我们可以将数据流机器的线程视为由多个同时执行的操作组成（例如，MIT Monsoon）。

## 5.3 论文讨论

接下来，我们将简要讨论本章转载的论文。

### 5.3.1 Dennis 和 Misunas 的“基本数据流处理器的初步架构”[6]

Dennis 和 Misunas 的论文是第一篇描述基于数据驱动执行原则的完整处理器架构的论文。这里，程序是一个数据流程序，而他们提出的方案是设计一个能够执行数据流程序的处理器。然而，这并非最早对数据流执行原则的描述：Tomasulo 的算法 [18] 在 IBM 360/91 上实现，应用了数据流原则（当时并不称为数据流），从而设计出一种能够以数据驱动方式执行少量指令的硬件。但需要注意的是，这组指令并非以数据流程序的形式呈现，而是通过按顺序执行一个更传统的控制流程序动态生成的。

### 5.3.2 Arvind 和 Nikhil 的《在 MIT TTDA 上执行程序》[3]

Dennis 及其同事的早期工作激发了世界各地多家机构对数据流处理器的深入研究，麻省理工学院也继续开展相关研究。 Arvind 和 Nikhil 的论文概述了 1987 年前后麻省理工学院（以及其他机构）在数据流领域的研究进展。

除了描述麻省理工学院标记令牌数据流架构 (TTDA) 计算机的架构之外，该论文还讨论了数据流计算的其他几个方面。

论文首先介绍了数据流语言 Id。如前所述，数据流的支持者认为，更传统的语言（或许是无意的）会引入操作顺序约束，而这些约束只会掩盖并行性。论文随后描述了如何将 Id 程序编译成数据流图，以及一种用于解释（即执行）以数据流图表示的程序的架构。

### 5.3.3 Smith 的《HEP 多处理器的架构与应用》[15]

Smith 的论文描述了 Denelcor 公司构建的异构元件处理器 (HEP) 的各个方面。 HEP 是早期使用多线程执行来处理“主”指令流的（公开文档记录的）机器的典型例子。更早的机器在某些处理环节中使用了多线程（例如，MIT TX-2 的输入/输出单元和 CDC 6600 的外围处理器）。在 HEP 之前，也曾提出过其他多线程机器（有些甚至已经建成），但这些机器的文档资料并不容易找到。

本文还介绍了一种新的同步机制（全空位）以及 Fortran 扩展，这些扩展允许编译应用程序以利用 HEP 的多线程架构和同步特性。

### 5.3.4 Tullsen 等人？ “利用选择：可实现的同步多线程处理器上的指令获取和执行”[20]

主流通用处理器的架构师并未认真考虑每周期（或每隔几个周期）切换线程的多线程架构，因为他们认为这种架构会牺牲单线程延迟来换取多线程吞吐量。此外，有限的芯片资源也让人质疑为支持多线程上下文而增加硬件投入的实用性，尤其是在单线程性能会受到影响的情况下。

宽发射机器的出现催生了一种新的多线程形式，即在同一周期内执行来自多个线程的操作。这种多线程形式可以在不影响单线程延迟的情况下提高多线程吞吐量（假设线程间干扰不会降低单线程延迟，例如，由缓存未命中增加导致的干扰）。20 世纪 90 年代，针对这种多线程类型提出了几种方案，既有针对静态调度宽发射机器的方案[12,21]，也有针对动态调度宽发射机器的方案。调度型宽发射机器[5,11,19,20,22]。

Tullsen 等人的论文讨论了在动态调度的超标量处理器上实现同步多线程时需要考虑的几个问题。他们展示了如何扩展超标量处理器的基本机制，以允许来自多个线程的指令同时处理。尽管该方案中的线程是不同的程序，但很容易看出它们也可以是同一个程序的不同部分。

## 5.4 展望未来

尽管数据流计算模型前景广阔，但它并未被主流处理器设计所采用。这其中有几个原因。首先，完全放弃传统的串行编程语言和指令集并不现实。其次，数据流模型“强大”的主要来源（没有人为的顺序约束，因此也不存在明确定义且可复现的状态）导致了一系列实际问题。其中最重要的问题或许是调试困难。[^2] Gajski 等人的论文 (8) 对这些局限性进行了探讨。因此，计算机架构师面临着这样一个问题：我们能否将数据流计算的一些原理应用于更实际的计算场景？到目前为止，这个问题的答案是肯定的。数据流范式持续启发着众多指令级并行（ILP）处理器的创新，这些处理器的目标是实现类似数据流的执行，但

[^2]：没有人工排序约束（即同步）的并行机器也面临着非常类似的问题。现代动态调度超标量处理器就是这种影响的一个例子，有些人称之为微型数据流机器。未来，数据流模型（结合合适的推测技术）很可能继续启发许多即将出现的并行利用创新。

未来的创新者可能会受益于对数据流概念及其演化的透彻理解。

数据流计算模型可能会发展到包含不同形式的推测，包括甚至可以消除数据流模型中唯一的排序约束——数据依赖性的推测。

未来的处理器也可能受到多线程技术的影响。回想一下，我们可以将多线程处理器视为并行处理器的实现（共享多个资源）。随着技术的发展，将“并行处理”能力集成到单个芯片上成为可能，因此需要考虑不同的“并行处理”功能集成方式。那些不会增加单线程延迟的多线程技术更有可能取得成功。理想情况下，多线程甚至可以用于提升单程序性能，而不仅仅是提高多线程吞吐量。这可以通过线程级推测来实现，正如一些近期研究项目所提出的那样[7, 10, 16, 17]。

## 5.5 参考文献


[1] A. Agarwal, B.-H. Lim, D. Kranz, and J. Kubiatowicz, "APRIL: A processor architecture for multiprocessing," Proceedings of the 17th Annual International Symposium on Computer Architecture, pp. 104-114, May 1990.  
[2] Arvind, K. Gostelow, and W. Plouffe, "An asynchronous programming language and computing machine," Tech. Rep. TR-114a, Department of Information and Computer Science, University of California, Irvine, Dec. 1978.  
[3] Arvind and R. S. Nikhil, "Executing a program on the MIT tagged-token dataflow architecture," IEEE Transactions on Computers, 39(3):300-318, Mar. 1990.  
[4] D. E. Culler and Arvind, "Resource requirements of dataflow programs," Proceedings of the 15th Annual International Symposium on Computer Architecture, Pp. 141-150, May 1998.  
[5] G. E. Daddis, Jr. and H. C. Torng, "The concurrent execution of multiple instruction streams on superscalar processors," International Conference on Parallel Processing, pp. I:76-83, Aug. 1991.  
[6] J. B. Dennis and D. P. Misunas, "A preliminary architecture for a basic dataflow processor," Proceedings of the 2nd Annual Symposium on Computer Architecture, pp. 126-132, Dec. 1974.  
[7] P. K. Dubey, K. O'Brien, K. O'Brien, and C.
Barton, "Single-program speculative multithreading (SPSM) architecture: Compiler-assisted fine-grained multithreading," Proceedings of the Conference on Parallel Architectures and Compilation Techniques, pp. 109-121, June 1995.  
[8] D. Gajski, D. Padua, D. Kuck, and R. Kuhn, "A second opinion on data flow machines and languages," IEEE Computer, pp. 58-69, Feb. 1982.
[9] J. R. Gurd, C. C. Kirkham, and I. Watson, "The Manchester prototype dataflow computer," Communications of the ACM, vol. 28, 1985.  
[10] L. Hammond, M. Willey, and K. Olukotun, "Data speculation support for a chip multiprocessor," Proceedings of the 8th International Conference on Architectural Support for Programming Languages and Operating Systems, pp. 58-69, Oct. 1998.  
[11] H. Hirata, K. Kimura, S. Nagamine, Y. Mochizuki, A. Nishimura, Y. Nakase, and T. Nishizawa, "An elementary processor architecture with simultaneous instruction issuing from multiple threads," Proceedings of the 19th Annual International Symposium on Computer Architecture, pp. 136-145, May 1992.  
[12] S. W. Keckler and W. J. Dally, "Processor coupling: Integrating compile time and runtime scheduling for parallelism," Proceedings of the 19th Annual International Symposium on Computer Architecture, Queensland, Australia, pp. 202-213, 1992.  
[13] G. M. Papadopoulos and D. E. Culler, "Monsoon:
An explicit token-store architecture," Proceedings of the 17th Annual International Symposium on Computer Architecture, pp. 82-91, May 1990.  
[14] T. Shimada, K. Hiraki, K. Nishida, and S. Sekiguchi, "Evaluation of a prototype data flow processor of the SIGMA-1 for scientific computations," Proceedings of the 13th Annual International Symposium on Computer Architecture, pp. 226-234, June 1986.  
[15] B. Smith, "Architecture and applications of the HEP multiprocessor computer system," Proceedings of the International Society for Optical Engineering, pp. 241-248, 1982.  
[16] G. S. Sohi, S. E. Breach, and T. N. Vijaykumar, "Multiscalar processors," Proceedings of the 22nd Annual International Symposium on Computer Architecture, pp. 414 425, June 1995.  
[17] J. G. Steffan and T. C. Mowry, "The potential for using thread-level data speculation to facilitate automatic parallelization," Proceedings of the 4th International Symposium on High-Performance Computer Architecture, Feb. 1998.  
[18] R. M. Tomasulo "An efficient algorithm for exploiting multiple arithmetic units," IBM Journal of Research and Development, pp. 25-33, Jan. 1967.  
[19] D. M. Tullsen, S. J. Eggers, and H. M. Levy, "Simultaneous multithreading: Maximizing on-chip parallelism," Proceedings of the 22nd Annual International Symposium on Computer Architecture, pp. 392-403, June 1995.  
[20] D. M. Tullsen, S. J. Eggers, J. S. Emer, H. M. Levy, J. L. Lo, and R. L. Stamm, "Exploiting choice: Instruction fetch and issue on an implementable simultaneous multithreading processor," Proceedings of the 23rd Annual International Symposium on Computer Architecture, pp. 191-202, May 1996.  
[21] A. Wolfe and J. P. Shen, "A variable instruction stream extension to the VLIW architecture," Proceedings of the 4th International Conference on Architectural Support for Programming Languages and Operating Systems, pp. 2-14, Apr. 1991.  
[22] W. Yamamoto and M. Nemirovsky, "Increasing superscalar performances through multistreaming," Proceedings of the Conference on Parallel Architectures and Compilation Techniques, pp. 49-58, June 1995.
