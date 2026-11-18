# Chapter 10: Recent Implementations and Future Prospects

 * Architecture of the Pentium Microprocessor -------------------- 649  
  *D. Alpert and D. Avnon*  
 * Tuning the Pentium Pro Microarchitecture ---------------------- 660  
  *D. B. Papworth*  
 * The Microprocessor Today -------------------------------------- 668  
  *M. Slater*  
 * The Future of Microprocessors --------------------------------- 681  
  *A. Yu*  

## 10.1 Introduction

In this section, we continue to look at improvements in base technology that have enabled recent architectural improvements in computers (we define "recent" to mean occurring in the last ten years). We also examine implementation aspects of different types of computers and cost issues. Without a good understanding of cost, implementations will not be economically successful. In the extreme, this can mean the cancellation of projects or even the failure of the business. Without a good understanding of implementation issues, architectural evaluations risk irrelevance. The lack of understanding of the marketplace, business issues, costs, and implementation issues results in many more project failures than architectural naïveté. Unfortunately, marketing, business, and economics are largely beyond the scope of this reader; this section attempts to give merely a flavor of these subjects. We believe all engineering students should take courses to gain an overview of economics, business accounting and finance, marketing, and business organization. Those who have already graduated are well advised to study these subjects on their own time. A quick overview of many important concepts can be found in The Ten-Day MBA by Steven Spilbiger [21].

## 10.2 Recent Microprocessor Design

For our discussion, we break the microprocessor design space into three segments: high-performance microprocessors, embedded volume microprocessors, and embedded high-performance microprocessors.

### 10.2.1 High-Performance Microprocessors

There have been a number of good papers published in the last decade covering the implementation of various high-performance microprocessors. We have selected one paper on the Pentium and one on the Pentium Pro as examples of in-order and out-of-order superscalar processors. Because they both implement a CISC instruction set, they generally have to deal with most of the issues of a RISC microprocessor design plus more. Also, by choosing two papers on very different microprocessors implementing the same instruction set, the contrast between the two implementations becomes more apparent. Other interesting recent CISC implementations include the IBM S/390 G5 Microprocessor [20] and x86 microprocessors from AMD [15]. There are currently many different RISC architectures, all with good implementations. We refer the reader interested in implementations of specific architectures to these papers on the Compaq Alpha [4, 91, Sun UltraSparc [14, 22], HP PA-RISC [101, and MIPS [24] microprocessors, as well as a book on the IBM/Apple/Motorola Power PC [23].

### 10.2.2 Embedded Volume Microprocessors

High-volume embedded processors are a very cost-sensitive segment of microprocessor design. The key principle in embedded design dates to the eighteenth century:

A penny saved is a penny earned.  
-Benjamin Franklin

Embedded microprocessors only need to implement enough computing power to run their particular appli-cation; any more functionality is wasted silicon, power, and money. This leads to the 8-bit microprocessor (with 4-bit external bus) still being the highest volume microprocessor in the embedded marketplace [18]. Ben Franklin's maxim also applies at the embedded system level-this pushes system functionality such as EPROM, counters and timers, serial interfaces, and so forth, onto the embedded microprocessor itself to reduce the system part count and hence cost. (There is lots of room left over for system components when you are implementing a single-issue 8-bit microprocessor.)

### 10.2.3 Embedded High-Performance Microprocessors

High-performance embedded microprocessors often perform long and repetitive numeric computations.
This means that complex mainstream techniques, such as sophisticated branch prediction and dynamic scheduling, are not as important in the embedded space. It also means that DSPs are more amenable to architectures such as very long instruction word (VLIW) or vector. One of the more well-known DSPs is the Phillips Trimedia VLIW media processor [19]. Other more-conventional high-performance embedded microprocessors include those in the StrongARM family [11].

## 10.3 Discussion of Papers on Microprocessor Design

In this section, we present two examples of recent high-performance microprocessor design.

### 10.3.1 Alpert and Avnon's "Architecture of the Pentium Microprocessor" [1]

This paper describes the architecture and implementation of the Pentium microprocessor as well as some of its design tradeoffs. Although the Pentium originally shipped at 60 MHz in a 0.8-um BiCMOS technology, shrinks and modifications have enabled it to run at almost 4X that clock rate in 0.35-um processes. This is a very wide range of performance, thanks to Moore's Law and some redesign.

Like some contemporaneous RISC processors, the Pentium is a dual-issue superscalar with an on-chip FPU, branch prediction, and write-back caches. However, the Pentium also had to maintain backward compatibility with the x86 architecture. This affected the architecture in several ways. For example, it needed a dual port data cache, because the x86 architecture has very few registers, and many instructions have an operand in memory. Besides handling all the x86 CISC instructions (including transcendental functions in hard-ware), it also had to correctly execute self-modifying code. The Pentium had a transistor budget almost 700 times larger than the 8080. It achieved a 2X speedup for integer code and a 5X speedup for floating-point vector code over the previous 1486 design when running at the same clock frequency.

### 10.3.2 Papworth's "Tuning the Pentium Pro Microarchitecture" [17]

As we discussed earlier, computer instruction-set architecture evolution tends to make the surviving architecture one of the "least fit." However, because the architecture is the most successful, it will be manufactured in volumes that dwarf any other high-performance microprocessor. (Very low cost microprocessors, such as those that play music in some greeting cards, are manufactured in much higher volumes than high-performance microprocessors. However, because of their low cost-sometimes less than one dollar-and the higher prices for high-performance microprocessors, the high-performance microprocessors still result in more total revenue.) Because the processor is manufactured in volumes that dwarf all other high-performance microprocessors combined, much larger amounts of money can be made available for development of that microprocessor family. This can be used to overcome the shortcomings that will be inherent in the most successful high-performance instruction-set architecture. Combined wit- phe enormous manufacturing economies of scale, this allows the most successful microprocessor to still be a price/performance leader even with a very complex design.

A case in point is the P6 design core that first appeared in the Pentium Pro and later in the Pentium II, Celeron, and Xeon processors. This core is the most successful machine with full out-of-order issue (including register renaming) ever built. One of the difficulties in implementing a dynamically scheduled machine is that it is very difficult to design and to verify. Thus, although it can result in significant performance improvements in dealing with unpredictable memory latency because of cache misses, the potential delays to project completion, and the increase in design expense are often not justifiable unless the design is produced in very large quantities.

This paper is interesting because it primarily describes the architecture design process for the Pentium Pro instead of describing just the final implementation. The design process included examining the tradeoffs in using a higher clock frequency with less work done each cycle and a resulting larger CPI. Like the designers of the DEC Alpha (and Seymour Cray before them), they found that raw clock speed is a powerful lever, so they chose a more heavily pipelined design than they had originally expected to. Not mentioned in the paper is that for people who know very little about computers (such as consumers and the management of some corporations), clock speed also serves as an even more meaningless indicator of processor speed than MIPS or MFLOPS. Thus, a highly pipelined design may be easier to sell to both corporate management and the public than a higher performance design with a lower clock frequency.

Another interesting feature of the Pentium Pro is that it combined two chips in a larger package. This allowed its manufacturing cost to be much less than if it consisted of a single chip, and this enabled it to be launched into the marketplace sooner. Although the two chips were more expensive than a simpler one-chip design (like the Pentium), it was initially targeted at server CPU applications where a more-expensive CPU was still a reasonable fraction of the system cost.

Combined with the Pentium in the same process, this enabled coverage of a greater range of the product space. The Pentium Pro design was introduced in 0.6-um technology at 150 MHz; derivatives of this design (the Pentium III) are already available at 550 MHz in
0.25-um technologies.

## 10.4 Current Trends and Future Prospects

As we have seen in earlier chapters, the primary driver of progress in computing technology is lithographic scaling of devices according to Moore's Law. This raises the question of how much longer Moore's Law will hold and what will eventually break it. There are at least four potential limits to Moore's Law: device characteristics, Lithography, design complexity, and fabrication cost.

### 10.4.1 Device Characteristics—A Physical Limit

Studies of extremely small transistors in laboratories have shown that it can be beneficial to scale CMOS transistors down to gate lengths of 0.025 um [8]. However, starting around the 0.1-um generation, if improved device performance is to be obtained with scaling, the gate oxide thickness must become so thin that electrons can tunnel through it. This results in a gate leakage current. Obviously, for some applications, such as DRAMs, this is not acceptable, but these applications can often live with lower performance devices. For logic applications, small gate currents can be taken into account in the circuit and logic design process. In this sense, MOS transistors will become more like bipolar devices, which have a base current when they are conducting!

The other important "device" in an integrated circuit are the wires [2]. Scaling of wires results in wires with more resistance and higher capacitance. Thus the inherent RC time constant of a wire itself will become a bigger and bigger factor of an integrated circuit's speed. Changes to copper metalization (for lower resistance wires) and low-K dielectrics (for lower capacitance wires) provide one-time gains on the order of a factor of two each. Because the resistance of copper is quite low and the dielectric constant of low-K dielectrics are near physical limits, further gains of this type should not be expected. The net result of increased wire time constants is somewhat slower performance and increased design complexity.
Architectural techniques (such as [16]) that can mitigate long on-chip wire delays are important areas for future research.

###  10.4.2 Lithography—A Technological Limit

The Rayleigh limit states that the minimum feature size patternable by light is proportional to its wavelength. To pattern features smaller than 0.1 um, very short wavelengths of light will be required. Unfortunately, most materials absorb these wavelengths, so conventional optical reduction and focusing systems using lenses are not possible. Three lithography options are possible: soft X-rays (about 1-nm wavelength), extreme ultraviolet light (at 13 nm, roughly halfway in logarithmic terms between the deep UV in use today and soft X-rays), and electron beams. Although all of these approaches are quite different from techniques in use today, the extreme-UV approach is the closest to what we are doing today and also potentially has throughputs that could be economically viable. An excellent reference on the future of lithography is Geppert's "Semiconductor Lithography for the Next Decade" [6]. More recently three National Labs, several companies, and Gordon Moore have announced a new "Limited Liability Extreme-UV Lithography Venture." The announcement of and information on this organization can be found via the online companion to this reader.

### 10.4.3 Design Complexity-An Implementation Limit

Both design-team size and the verification complexity of large designs can be examples of design complexity limits.

The 8008 was designed in a 10-um process by two designers [12] and an architect. Recent flagship microprocessor designs have required some 200 designers. Thus, the number of people needed to implement a microprocessor has been increasing in rough inverse proportion to the feature size of the process. Managing such a large team of people to design anything is an enormous design challenge. It also requires large amounts of structure in a design so that aspects of the design can be managed by leaders of smaller teams. For example, in the Pentium paper [1], the authors state that the two parallel execution pipes were given the names U and V, because these were the first two adjacent letters of the alphabet that had not already been used as a box (design subunit) name. This means that there were at least ten boxes in the design, and probably more. One result of adding large amounts of structure is that optimizations between boxes of a design may not be obvious to many people. Also, because the interfaces between boxes must be frozen early in the design, later changes in the machine structure may not be possible without unacceptable delays in the project. Besides having many small teams operate in parallel, such large designs are often pipelined into design stages. These stages consist of overall architecture, microarchitecture, logic design, circuit design, and layout (as well as verification at each stage sometimes done by a separate parallel team). In this kind of design pipeline, negative effects at later stages (e.g., a failure to meet design cycle time goals on a particular unit) are very hard to fix by backing up the pipeline without unacceptable delays in the project. It is even more unlikely that new better ideas at any stage can be implemented once that phase is complete-this would also delay the project.

Today's microprocessor designs are much more complex than were early designs. This is particularly true in the case of out-of-order issue multiprocessors. Verification of simple in-order issue unprocessors can be done to acceptable quality by the brute force approach of trying to simulate all possibilities. However, with out-of-order issue multiprocessors, the state space is just too large to cover in total by simulation. Formal verification methods can help, but verification of the design is still in itself an enormous task. If future microprocessors were constructed out of a number of simpler multiprocessor-capable cores or already-verified out-of-order issue processors, the design verification problem would also be simplified [7].

As the number of people required for the design and verification of a microprocessor increases, the development cost increases. This raises the volumes necessary for profitable development of microprocessors and hence leads to further consolidation in the industry.

### 10.4.4 Fabrication Cost-An Economic Limit

Gordon Moore has stated on several occasions [13] that the capital cost of building a fab is increasing at a rate that is roughly inversely proportional to the feature size. The cost of building a 0.25-um fab capable of starting 5,000 wafers per week is now about $1.3 bil-lion. If this trend continues, the cost of a 0.03-um fab a decade from now would be over $10 billion. Very few companies could justify this scale of investment (most semiconductor company revenues are less than $10 billion per year!) Thus fewer and fewer companies will be able to build fabs with the necessary economies of scale to be competitive. This will lead to further consolidation in the number of semiconductor companies and products.

The high cost of semiconductor fabrication equipment also leads to increased process development cost. This is because process development must be done with some of the same equipment that will eventually be used for production, and developing a process takes time (e.g.) two years. The very-expensive equipment depreciates while it is being used for process development, and this expense alone can make developing a different process prohibitively expensive (eg., $100 million). This puts an emphasis on evolutionary versus revolutionary process development and leads to a smaller number of processes in the world (e.g, only a basic CMOS, which would have the highest volume versus a BiCMOS or GaAs process).

### 10.4.5 Prognosis

Moore's Law should continue to hold through the year 2007 (81. But eventually the scaling curve will saturate, and improvements will occur less frequently, such as doubling instead of quadrupling every three years. But before then, the implications of increasing fabrication cost and design complexity will result in many fewer fabrication lines than we have today (each with higher capacity and much more expensive) controlled by fewer semiconductor companies producing a smaller number of more powerful parts on a smaller number of more powerful fabrication processes. In other words, there will be a loss of diversity in designs (e.g., fewer microprocessors developed each year) and in processes (e.g., convergence on a standard high-performance CMOS process) resulting from economic necessities.

## 10.5 Discussion of Papers on Current Trends and Future Prospects

In this section we introduce two papers that describe current trends and future prospects in microprocessor engineering.

### 10.5.1 Slater's "The Microprocessor Today" [18]

Michael Slater is the founder and editorial director of Microprocessor Reports. In those reports and in this article, he tries to consider all the factors that lead to success or failure of various industry approaches.

One of the more important considerations is manufacturing cost versus selling price. This paper contains estimates of various microprocessors' manufacturing costs as well as their selling prices. In one table, he estimates that Intel microprocessors typically cost less to manufacture but sell for more than the AMD or Cyrix parts. This goes a long way toward explaining Intel's dominance of that market and also explains how it can fund investments in many billion-dollar fabs at the same time. RISC processors have much higher ratios of sales prices to manufacturing cost, but this does not include design costs which, because of the RISC processor's much smaller volumes, are much larger per microprocessor. Unfortunately for computer architects, issues like these (and object code compati-bility) are more important in the marketplace than architectural strength and purity are.

Although architectural compatibility is very important in the desktop and large system markets, the embedded marketplace has different constraints. Here the primary focus is reducing the cost as much as possible, often by integration of other system functions on the microcontroller. Because object-code compatibility is not such a large issue in this market, "embedded MIPS" are much more of a commodity market, with correspondingly lower prices and margins.

### 10.5.2 Yu's "The Future of Microprocessors" [25]

This article examines the impact of continued scaling and Moore's Law on future microprocessor performance. In a related article in 1989 [5], performance projections were made for the coming decade. These previous projections underestimated microprocessor performance circa 1995 by a factor of four by underestimating improvements in cycle time and in IPC. Because the fabrication cost, design cost, and design verification tasks are all increasing in proportion to feature size reductions, these factors may pose the most significant barriers to continued microprocessor evolution in the future.

## 10.6 References

A large number of interesting papers on future directions for microprocessor architecture appeared in a special issue of IEEE Computer "Billion-Transistor Architectures," edited by Burger and Goodman [3]. Please also see the Web companion to this reader for additional Web references.

[1] D. Alpert and D. Avnon, "Architecture of the Pentium microprocessor," IEEE Micro, 13(3):11-21, 1993.  
[2] M. T. Bohr, "Interconnect scaling— the real limiter to high-performance ULSI," International Electron Devices Meeting Technical Digest, pp. 241-244, 1995.  
[3] D. Burger and J. R. Goodman, "Billion-transistor architectures," IEEE Computer, 30(9):46-48, 1997.  
[4] J. H. Edmondson, P. Rubinfeld, and R. Preston, "Superscalar instruction execution in the 21164 Alpha microprocessor," IEEE Micro, 15(3):33-43, 1995.  
[5] P. P. Gelsinger, P. A. Gargini, G. H. Parker, and A. Y. C. Yu, "Microprocessors circa 2000," IEEE Spectrum, 26(10):43-47, Oct. 1989.  
[6] L. Geppert, "Semiconductor lithography for the next decade," IEEE Spectrum, 33(4):33-38, Apr. 1996.  
[7] L. Hammond, B.A. Nayfeh, and K. Oluktun, "A single-chip multiprocessor," IEEE Computer, 30(9): 79-85, 1997.  
[8] H. Iwai, "CMOS technology-Year 2010 and beyond," IEEE Journal of Solid-State Circuits, 34(3):357-366, 1999.  
[9] R. Kessler, "The Alpha 21264 microprocessor," IEEE Micro, 19(2):24-36, 1999.  
[10] A. Kumar, 'The HP PA-8000 RISC CPU," IEEE Micro, 17(2):27-32, 1997.  
[11] T. Litch and J. Slaton, "StrongARMing portable communications," IEEE Micro, 18(2):48-55, 1998.  
[12] S. Mazor, "The history of the microcomputer-Invention and evolution," Proceedings of the lEEE, 83(12):1601-1608, 1995.  
[13] G. Moore, "Nanometers and gigabucks," Proceedings of Hot Chips VII, Aug. 1995.  
[14] K. B. Normoyle et al., "UltraSparc Ili: Expanding the boundaries of system on a chip," IEEE Micro, 18(2):14-24, 1998.  
[15] S. Oberman, G. Favor, and F. Weber, "AMD 3D Now! technology: Architecture and implementations," IEEE Micro, 19(2):37-48, 1999.  
[16] S. Palacharla, N. P. Jouppi, and J. E. Smith, "Complexity-effective superscalar processors," Proceedings of the 24th International Symposium on Computer Architecture, pp. 206-218, 1997.  
[17] D. B. Papworth, "Tuning the Pentium Pro microarchitecture," IEEE Micro, 16(2):8-15, 1996.  
[18] M. Slater, "The microprocessor today," IEEE Micro, 16(6):32-44, 1996.  
[19] G. A. Slavenburg, S. Rathnam, and H. Dijkstra, "The Trimedia TM-1 PCI VLIW mediaprocessor," Proceedings of Hot Chips VIII, Aug. 1996.  
[20] T. J. Slegel et al., "IBM's S/390 G5 microprocessor," IEEE Micro, 19(2):12-23, 1999.  
[21] S. Spilbiger, The Ten-Day MBA. New York: William Morrow, 1993.  
[22] M. Tremblay and J. M. O'Connor, "UltraSparc I: A four-issue processor supporting multimedia," IEEE Micro, 16(2):42-50, 1996.  
[23] S. Weiss and J. E. Smith, POWER and PowerPC: Principles, Architecture, and Implementation, San Francisco, CA: Morgan Kaufmann, 1994.  
[24] K. C. Yeager, "The Mips R10000 Superscalar microprocessor," IEEE Micro, 16(2):28-40, 1996.  
[25] A. Yu, "The future of microprocessors," TEEE Micro, 16(6):46-53, 1996.  