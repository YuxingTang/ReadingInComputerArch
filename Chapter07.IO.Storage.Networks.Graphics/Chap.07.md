# Chapter 7: I/O：存储系统、网络和图形

 * A Sequencing-Based Taxonomy of I/O Systems and Review of Historical Machines  ------ 451
  *M. Smotherman*
 **STORAGE SYSTEMS**
 * An Instroduction to Disk Drive Modeling --------------------------------------------- 462
  *C. Ruemmler and J. Wilkes*
 * A Case for Redundant Arrays of the Inexpensive Disk (RAID) -------------------------- 474
  *D.A. Patterson, G. Gibson and R.H. Katz*
 **NETWORKS**
 * Ethernet: Distributed Packet Switching for Local Computer Networks ------------------ 482
  *R.M. Metcalfe and D.R. Boggs*
 * A Survey of Wormhole Routing Techniques in Direct Networks -------------------------- 492
  *L.M. Ni and P.K. Mckinley*
 **GRAPHICS**
 * RealityEngine Graphics -------------------------------------------------------------- 507
  *K. Akeley*

&emsp;&emsp; 在本章中，我们首先从系统环境中I/O地位的经济基础入手。然后，我们将现代个人电脑（PC: Personal Computer）的I/O系统作为应用I/O概念的案例研究。最后，我们以在I/O演变和分析中的一些关键论文做结论。Hennessy和Patterson的《计算机体系结构：量化方法》[2]中的I/O章节已经详细介绍了I/O的基本概念，我们在此不再赘述。

## 7.1 I/O经济学

&emsp;&emsp; 计算机的I/O系统通常占其成本的大部分。例如，即使在700美元的PC（包括显示器）中，处理器和存储加起来的成本也可能不到150美元。最昂贵的组件是显示器，在这样的系统中，它本身的成本可能为150美元。磁盘也相当昂贵，成本接近100美元。CD-ROM驱动器、软盘、图形加速器、调制解调器、主板、机箱和电源可以占剩余的200美元。

&emsp;&emsp; 普遍的看法是，I/O系统的性能已经落后了几十年，并且会导致计算机系统性能即将陷入困境，但它仍然很少受到学术界的关注。I/O问题确实很少受到学术界的关注，而I/O设备的性能通常会拖累整个系统的性能。但是，根据上述对低成本PC组件的经济分析，您可以开始理解为什么会这样。更快的I/O功能通常需要花费更多钱。由于I/O已经是主要的系统成本，I/O速度加倍（可能加倍I/O成本和接近加倍系统成本）的边际收益并不好。相反，提供更快的CPU（相对较便宜的组件）更有意义，这样较慢（且更昂贵）的I/O系统就会饱和。举一个更具体的例子，磁盘成本为100美元，提供的存储容量超过普通PC用户的需求，因此目前在台式机上使用RAID在经济上是不合理的。

&emsp;&emsp; 因此，I/O中一些更重要的趋势涉及在给定的性能水平下降低其成本，或在给定的成本水平下提高其性能（以容量或速度而言）。由于摩尔定律不适用于连接器上的线缆，因此用半导体元件替代机电元件也是I/O系统的一个重要趋势。例如，通用串行总线 (USB)最近出现的原因之一是用I/O设备中较少数量的导线取代了大量的导线，这些导线由高速半导体驱动和感应（其中一些设备过去非常简单，几乎是无源的，比如键盘和鼠标）。磁盘存储密度不断增加的另一个关键因素是越来越高速度的接口电子设备极其越来越复杂功能。磁盘技术的最新进展，如部分响应最大似然(PRML: Partial Response Maximum Likelihood)[11]，要求磁盘读取通道中的数字信号处理器(DSP: Digital Signal Processor)来分离彼此重叠的位。再举一个例子，阴极射线管(CRT: electroCathode-Ray Tube)显示器的像素数受到机电因素的限制，而超大显示器的成本极其昂贵（例如，2,000×2,000像素的显示器目前的成本为35,000美元）。通过图形加速器对计算机生成的图像进行全屏抗锯齿处理，可以提高显示器的视觉质量，使其与实际分辨率的2到3倍相当。但是，这需要更大的帧缓冲存储器来实现。随着时间的推移，半导体元件不断扩展缩放，而机电元件却没有，因此，使用额外的半导体资源来生成全屏抗锯齿图像比购买更高分辨率的显示器更经济。实时抗锯齿技术最早在硅图公司真实引擎（Silicon Graphics RealityEngine）中得到应用，几年后，借助摩尔定律的优势，该技术很可能会移植到PC上。最后一个例子是，WebTV系统消除了每个用户的磁盘、CD-ROM、软盘和显示器，用共享远程磁盘替换了私有本地磁盘，借助用户电视上的CRT显示器替换了每个人的专用显示器。这节省了PC的很大一部分成本，并减轻了用户的系统管理责任。

## 7.2 PC I/O案例研讨

&emsp;&emsp; 桌面上或桌子侧面的PC是I/O系统的绝佳案例研究。现在大型计算机系统正在采用标准PC I/O总线的多个实例来降低其总系统成本，这一点尤其正确。出于两个原因，对于给定的功能级别，PC组件总是比专用的专有组件便宜。首先，PC组件是大批量生产的，并且受益于规模经济。其次，由于它们的设计符合行业标准，因此有许多不同的竞争制造商生产这些组件，其中许多制造商的财务利润很低或正在亏损。

&emsp;&emsp; 在下面的案例研究中，我们主要关注I/O总线。本节后面的论文以及Hennessy和Patterson的《计算机体系结构：量化方法》[2]的I/O章节很好地覆盖了I/O组件的讨论。I/O总线很重要，因为它们可能是高性能I/O设备和高性能CPU之间的瓶颈。这因为几个因素而加剧。首先，为了在第三方I/O设备（即图形加速器、视频采集卡、数据采集卡等）的开发中实现规模经济，相同的附加卡必须可以使用多年，即使PC系统每年都在变化。由于这些附加卡的产量可能小于基本系统的产量，因此开发成本需要分摊到几年内以降低可能的成本。这对于更专业的卡来说是一个更重要的因素；例如，实验室数据采集卡的进化速度将低于大批狂热游戏玩家购买的PC图形加速器。因此，通用I/O总线的发展往往相当缓慢，并且以向后兼容的方式发展。向后兼容意味着新的I/O总线将支持为同一总线系列设计的较旧的上一代I/O设备。

&emsp;&emsp; 当引入新总线时，增加的成本考虑往往会限制总线设计的性能余量。然而，随着时间的推移和总线的发展，总线上较旧的I/O设备的存在会降低其性能，使其与上一代总线的性能相当，并成为系统性能的瓶颈。

&emsp;&emsp; 当今PC上最重要的总线和接口是串行端口、并行端口、ISA、PCI、AGP、PCMCIA、USB、IDE和SCSI。我们简要概述了每个接口的一些有趣问题。有关更多详细信息，建议读者参阅众多PC参考书或在线指南。读者可以在网络伴侣上找到有用的在线指南的参考资料。一本关于这个主题的好书是Hans-Peter Messmer的《不可或缺的PC硬件书》[5]。

### 7.2.1 串口

&emsp;&emsp; 串行端口采用1969年开发的RS-232C（EIA: [电子工业联盟 Electronic Industries Alliance]推荐标准232C）。它支持低至110波特的通信速率，以支持旧的电传打字机设备。波特是位串行线路改变状态的频率的度量，波特单位以法国电报服务官员Jean Maurice Emile Baudot的名字命名。在十九世纪末，他发明了第一个统一长度的5位字母字符代码。最初的RS-232C标准仅支持高达20Kbaud的通信速率；但是，较新的设备通常可以在短距离内使用良好的电缆运行高达 115Kbaud。

### 7.2.2 并行端口

&emsp;&emsp; PC上的并行端口源自Centronics打印机端口。该端口由8位并行数据输出组成，最初由Centronics在20世纪70年代定义，后由Epson改进。该接口可实现的最大数据传输速率约为150Kbytes/s，但对软件的要求极高。到20世纪80年代末，外部I/O设备（如扫描仪）需要高速双向通信。为此，定义了两个标准（EPP[扩展并行端口: Extended Parallel Port]和ECP[扩展功能端口: Extended Capabilities Ports]），后来在1994年合并为IEEE 1284标准。此新标准允许高达1 Mbyte/s的双向通信以及在端口上以菊花链式来连接多个设备。

### 7.2.3 ISA总线

&emsp;&emsp; ISA总线源自1981年原始IBM PC的主要8位内部数据总线。在这台PC中，它用于将 CPU与内存和I/O设备连接起来。该总线具有20位地址字段（能够寻址高达1Mbyte）。第二代IBM PC/XT使用具有16位数据总线的Intel 8086，但仍保留相同的I/O插槽以实现兼容性。随着1984年基于 Intel 80286的IBM PC/AT（AT表示先进技术Advanced Technology）的推出，这些I/0插槽以向上兼容的方式进行了扩展。在原始8位PC卡插槽的原始62个触点旁边添加了第二组36个触点，以将数据总线扩展为16位。经过多年因时钟速度和时序不同而导致的混乱，接口被标准化为行业标准体系结构 (ISA: Industry Standard Architecture) 总线。总线规定以8.33MHz的最大时钟频率运行。这给出了16Mbytes/s的最大速度，但由于内存或慢速I/0设备需要等待周期，3Mbytes/s的速度更常见。ISA总线提供DMA传输以减少对CPU的需求。不幸的是，由于它源自原始PC的主总线，因此它要求每个适配卡从有限数量的中断请求(IRQ: Interrupt ReQuest)向量中选择一个。安装ISA卡最初需要在卡上设置跳线以选择地址和IRQ。这使得添加新的适配卡变得很棘手，因为它们可能与现有的卡冲突。对于Windows 95，Microsoft和Intel为ISA总线引入了即插即用（PnP: Plug-and-Play）扩展，允许用户无需设置跳线或开关即可安装扩展卡。20世纪90年代后期，许多用于低性能设备的扩展卡仍然只连接到8位总线的原始62个触点，以降低成本。

### 7.2.4 PCI 总线

&emsp;&emsp; 外设组件互连(PCI: Peripheral Component Interconnect)总线由Intel开发，于1993年引入Pentium系统。PCI总线具有32位宽的数据总线，可以以33MHz的速度运行。这使其峰值速度达到133Mbytes/s。地址和数据在相同的信号上多路复用，因此对于单字传输，这会立即将峰值速度减半。读取周期也需要一个周期来在总线中绕回，将速度降低到独立单字系统读取的峰值速率的三分之一。但是，PCI总线提供了突发（burst）传输能力，这使得较长传输的性能更接近总线的峰值速度（因为在这种情况下，地址周期之后会有多个数据周期）。PCI总线还允许I/O设备成为总线主控并控制数据突发的传输。

&emsp;&emsp; PCI总线的一大进步是，它通过包含传输缓冲区的芯片组实现了了和PC的处理器和内存解耦，并且具有自己的中断系统，该系统与处理器的IRQ解耦。传输缓冲区允许PCI总线在没有内存等待状态的情况下与处理器异步并行运行。这是获得接近峰值速率的传输速率的关键。它还允许内存和I/O系统（包括它们的时钟速度）的独立可扩展性。单独的中断结构使PCI芯片组电路能够处理卡的识别并与操作系统和BIOS协同工作，以自动设置兼容外围卡的资源分配。这使得安装新卡变得容易。已经开发了PCI总线的扩展，将其扩展到64位和66MHz；然而，在实践中它们仍然很少见。

### 7.2.5 AGP

&emsp;&emsp; 在显示视频或3-D图形时，图形加速器和其他I/O设备之间共享的PCI总线可能会成为瓶颈。为了加快图形处理速度并释放PCI总线以用于其他I/O，Intel于1997年推出了高级图形端口(AGP: Advanced Graphics Port)。AGP 类似于PCI总线的专用版本，专门用于图形加速器。AGP是一个端口而不是总线，因为它仅将（主）系统图形加速器连接到系统芯片组。由于它始终只由两个互连的设备组成，而不是2个到7个或更多，因此AGP上的电信号可以更快地运行。原始AGP以66MHz运行，是PCI总线速度的两倍。AGP2X在端口时钟的两个边缘传输数据，并提供高达533Mbytes/s的带宽。进一步加倍速度到AGP4X已经定义好了。

### 7.2.6 PCMCIA

&emsp;&emsp; 个人计算机存储卡国际协会(PCMCIA: Personal Computer Memory Card International Association)为信用卡大小的设备设计了一系列标准。最初，它被认为是一种为便携式设备提供额外内存的手段（1990年标准化的3.3毫米厚的I型外形）。这种外形后来扩展到5.0毫米的II型，可以支持插入式调制解调器卡和网络接口。最终的10.5毫米III型格式可以支持微型磁盘驱动器。在这三种格式中，II型插槽是最常见的。较大的插槽设计用于接受任何较小类型的卡。最初的规范涵盖了以ISA时钟频率（8.3 MHz）运行的8位和16位数据宽度的卡，而1996年的CardBus规范支持32位宽度和33MHz速度，与标准PCI总线相同。卡和接口还设计为可热插拔，因此在一个用户会话中可以以不同的方式使用有限数量的插槽。PCMCIA接口的一大优势是它允许通用调制解调器和其他标准设备在各种平台上使用，从而降低成本并增加产品多样性。在PCMCIA标准之前，不同的产品具有不同的接口；因此，任何插入式产品都是专用的、产量低、成本高且多样性有限。

### 7.2.7 USB

&emsp;&emsp; 通用串行总线(USB)提供最高12Mbits/s的带宽。这足以满足低性能PC外设（如指点设备、键盘、调制解调器和低分辨率、低帧率相机）的需求。USB设备通过树形拓扑连接到计算机，使用多端口集线器进行扩展。USB连接由差分数据对和两个电源连接组成。低功耗设备（如键盘或鼠标）可以从总线获取电源。USB设备包括支持设备热连接和断开连接的即插即用机制。USB通信包括发送数据包。它提供异步通信和等时通信，其中特定设备被授予使用部分可用总线带宽的保证。这种通信方式在多媒体应用中很重要，因为多媒体应用通常面临严格的延迟限制。USB的两个重要优点是它不需要用户打开PC机箱来添加外设，而且它价格低廉，因为它只有四根线。

### 7.2.8 FireWire/IEEE 1394

&emsp;&emsp; FireWire是一种高速外部串行接口，最初由Apple Computer开发，后来成为IEEE 1394标准。IEEE 1394使用由电源、接地、差分数据和频闪信号组成的六芯电缆。它目前支持高达800 Mbits/s的速度，这使其适用于高质量视频设备。与USB一样，IEEE 1394支持即插即用外围设备连接，为外围设备供电，并支持同步数据传输。由于速度更快，IEEE 1394的成本高于USB，因此速度较低的设备仍将使用USB而不是IEEE 1394。为了在串行线路上获得如此高的速度，IBEE 1394需要在线路的两端都安装快速且相当复杂的接口芯片。这是复杂硅片取代更昂贵的机电元件趋势的一个例子。

### 7.2.9 IDE

&emsp;&emsp; 1984年末，Compag与Western Digital、Imprimis和Seagate合作，开始开发智能驱动电子(IDE: Intelligent Drive Electronics)接口。在此之前，PC中的磁盘驱动器与其磁盘驱动器的接口非常低级，因为向每个磁盘驱动器添加智能控制器的成本过高。但是，最初的IDE设计存在许多不兼容性，因此在1989年，该标准被重新定义为AT附件(ATA: AT Attachment)标准。今天，IDE和ATA是同义词。IDE/ATA总线为16位宽，源自PC/AT中的16位数据总线。最初的IDE/ATA规范允许每个通道总共有两个驱动器，并支持高达8.3Mbytes/s的DMA速率。在EIDE（增强型IDE）和ATA-2 中，该速率扩展到16.6 Mbyte/s。（遗憾的是，不同的制造商对其兼容的标准驱动器使用了不兼容的名称。）ATA-2接口还通过添加称为ATA数据包接口（ATAPI: ATA Packet Interface）的附加协议功能扩展到CD-ROM和磁带驱动器。最后，最大性能在Ultra ATA/DMA-33代次中扩展至33.3Mbytes/s，在Ultra ATA/DMA-66代次中扩展至66.6 Mbytes/s。但是，即使在这一代中，它也没有实现拆分事务（待处理pended）协议[2]，因此典型性能可能明显低于峰值性能。最大IDE总线长度限制为18 英寸，因为它使用扁平（非双绞线）非屏蔽带状电缆。这种电缆价格低廉，但最初设计的传输速度仅为8. Mbytes/s。这意味着IDE仅在PC机箱内有用。

### 7.2.10 SCSI

&emsp;&emsp; 最初的小型计算机系统接口(SCSI: Small Computer System Interface)标准源自Shugart Associates系统接口(SASI)。SCSI-I于1986年被ANSI批准为标准X3.131-1986。它仅定义了一条8位宽的总线，通过50芯电缆以5MHz的最大传输速度运行。高级SCSI-2规范于1990年被ANSI批准。它定义了16位（“宽”）和10MHz（“快”）的扩展，并将总线上支持的最大设备数量从8个增加到16个。这需要一种新的高质量68芯双绞线电缆、改进的连接器和有源终端，从而提供更好的信号完整性。SCSI-2中还添加了命令排队，允许多个未完成的事务。这在具有多个设备的高性能系统中比IDE具有显著优势。最后，SCSI-2增加了命令增强功能，以便与各种设备（如可移动介质存储设备和扫描仪）一起操作。最近，出现了20MHz（“超”）SCSI设备。如果“窄”，则最大传输速率为20Mbytes/s，如果“宽”，则最大传输速率为40Mbytes/s。最大SCSI电缆长度与其速度成正比；5MHz系统可以支持6米长的电缆，而20MHz系统则限制为1.5米长的电缆。为了克服较高速接口缩短的电缆长度，在上述所有变体中都引入了差分SCSI（最大电缆长度为25米）。最新的80Mbyte/s Ultra2 SCSI需要此电缆，它使用低压差分信号，传输距离可达12米。

&emsp;&emsp; 由于它们专注于服务器等更高性能的系统，因此通常会发现SCSI主机适配器内置了对硬件实现的RAID的支持。对于高端系统，开发了一种称为单连接器连接(SCA: Single Connector Attachment)的变体，它使用80针连接器和电缆为68针接口增加功耗，允许热插拔和重新配置驱动器。由于其布线和接口更复杂，体积更小，SCSI磁盘通常比相同容量的IDE磁盘贵1.5-2倍。

## 7.3 总结

&emsp;&emsp; I/O设备将继续主导计算机系统成本，因为它们基于机电设备，其扩展速度与摩尔定律不同。因此，由于更高CPU性能的边际成本较低，因此其性能通常仍然是整个系统性能的限制，这在经济上仍然有意义。请注意，尽管磁盘容量一直与摩尔定律保持一致，但这主要是由光刻技术减小薄膜读/写头尺寸以及由光刻技术推动的电子器件改进所驱动的。但是，磁盘驱动器的最低成本扩展速度远低于其容量。例如，当前与三年前的磁盘具有相同物理大小的磁盘可以容纳四倍的数据，但它们的价格仍然大致相同。

&emsp;&emsp; 只要摩尔定律继续有效，用半导体元件取代越来越多的机电元件也将继续具有经济意义。这些例子包括高速串行互连、全屏抗锯齿和多台PC之间共享的I/O设备。

&emsp;&emsp; 最后，小批量专用I/O设备的开发必须在多年内分摊。此外，由于I/O设备占系统成本的很大一部分，许多用户从升级计算机系统的同时保留一些现有的I/O设备中受益。这两个因素以及其他因素使I/O标准非常重要。它还施加了来自市场的压力，要求以向后兼容的方式改进I/O标准。

## 7.4 所收录论文的讨论

&emsp;&emsp; 我们收录的论文首先介绍了I/O技术的历史分类，然后介绍了存储、网络和图形方面的关键论文。

### 7.4.1 Smotherman《基于排序的I/O系统分类和历史机器回顾》[11]

&emsp;&emsp; 在计算机的整个历史中，使用了各种各样的I/O技术。Smotherman提出了一种I/O分类法，并提供了丰富的历史参考资料和每种机器的简要概述。这些技术本质上是基础技术，在当今的机器中重新出现，包括I/O结构和处理器。例如，1957年的MIT TX-2有33个程序计数器，是当今多线程机器的先驱。同样，PC也一直在I/O进化阶梯上不断攀升，最近的智能I/O (I2O) 提案在 PC中引入了I/O处理器，这种处理器自20世纪50年代和60年代以来就已出现在大型处理器中。

### 7.4.2 Ruemmler和Wilkes的《磁盘驱动器建模简介》[10]

&emsp;&emsp; 自20世纪50年代以来，磁盘驱动器一直是计算机系统的一部分：在不同时期，有人提出半导体或新奇技术将取代或取代磁盘驱动器。然而，几十年来，磁盘驱动器在成本、容量、可靠性和性能方面不断扩大，仍然是计算机系统的关键组件。

&emsp;&emsp; Ruemmler和Wilkes是理解和建模最新磁盘驱动器的绝佳参考。首先，它描述了磁盘驱动器各个组件的操作，从I/O设备驱动程序、SCSI总线和磁盘控制器到磁盘驱动器内的缓存、磁盘上的数据布局，最后是磁头定位效果。然后，它开发了一系列复杂程度不断增加的磁盘驱动器操作模型，并根据实际测量数据验证了这些模型。本文提供的对磁盘驱动器操作的良好理解对于那些对RAID等高级I/O体系结构感兴趣的人至关重要。

### 7.4.3 Patterson、Gibson和Katz的《廉价磁盘冗余阵列(RAID)案例》[9]

&emsp;&emsp; 20世纪70年代的Cray-1等超级计算机可以将文件条带化分布到多个磁盘上，从而提高性能。然而，直到最近，磁盘才变得足够便宜，可以将文件分布在多个磁盘上，这才得到更广泛的应用。
这第一篇RAID论文超越了条带化，通过使用RAID提供了一系列性能和可靠性改进。

### 7.4.4 Metcalfe和Boggs的《以太网：本地计算机网络的分布式分组交换》[6]

&emsp;&emsp; 计算机网络主要有两种类型：计算机系统之间的网络，如局域网；大型计算机系统内的网络，如多计算机mesh网络。Metcalfe和Boggs的经典论文介绍了以太网，它是第一个成功的局域网，也是分布式系统发展的关键部分。尽管最初的开发速度为3Mbits/s，但以太网很快扩展到10Mbits/s。最近，它已扩展到100Mbits/s甚至1Gbits/s，以及交换（即点对点）配置。

7.4.5 Ni和McKinley的《直接网络中的虫洞路由技术调查》[8]

&emsp;&emsp; Ni和 McKinley是当今大型多处理器中常见的互连网络的绝佳参考。它首先概述了直接网络拓扑、路由、流量控制和交换。然后描述了虫洞路由，其中​​数据包的各段以流水线方式通过网络。还介绍了与确定性、自适应和可重构网络路由中的死锁相关的问题。

7.4.6 Akeley的《真实引擎图形》[1]

&emsp;&emsp; Akeley的论文介绍了20世纪90年代早期最重要的图形体系结构之一。该体系结构当时被实现为图形超级计算机。然而，随着技术和集成的进步，图形超级计算机作为超级计算机已经迁移到工作站，最近又迁移到PC图形体系结构。

&emsp;&emsp; 真实引擎（Reality Engine）引入了许多计算机图形硬件的重大进步。RealityEngine提供了每秒超过一百万个三角形的性能，结合了三线性mip-mapped纹理映射和全场景抗锯齿。（抗锯齿可消除或减少简单像素基图形中的阶梯状、锯齿状和爬行状。）前两个功能在当今的工作站和PC中已变得很常见，而抗锯齿可能会开始出现在下一代工作站图形中。为RealityEngine及其同类产品开发的GL图形语言已发展成为OpenGL标准，确保当今许多图形加速器以类似的方式实现相同的功能。

## 7.5 参考文献

&emsp;&emsp; 可以在Mee、Daniel和Clark撰写的《磁记录：第一个100 年》[4]中找到有关磁盘和磁带技术的良好历史概述。最近有关磁盘技术的文章包括Wood和Hodges[13]以及 Tsang、Chen 和Yogi[12]。

&emsp;&emsp; 可以在Molnar、Eyles 和 Poulton[7]中找到其他高端图形体系结构方法。McCormack等人在[3]中介绍了Neon单芯片图形加速器。有关图形体系结构的论文的其他来源包括 SIGGRAPH/Eurographics 图形硬件研讨会的最新论文集。I/O Web参考资料在读者的配套网站上提供。

[1]K. Akeley, "RealityEngine graphics," SIGGRAPH '93 Proceedings, pp. 109-116, 1993.
[2]J. L. Hennessy and D. A. Patterson, Computer Architecture: A Quantitative Approach, San Francisco, CA: Morgan Kaufmann, 2nd ed. 1996.
[3]J. McCormack et al., "Implementing Neon: A 256-Bit graphics accelerator," IEEE Micro, 19(2):58-69, 1999.
[4]C. D. Mee, E. D. Daniel, and M. H. Clark. Magnetic Recording: The First 100 Years. Piscataway, N. J.: IEEE Press, 1999.
[5]H. P. Messmer, The Indispensable PC Hardware Book, Reading, MA: Addison-Wesley-Longman, 3rd ed. 1997.
[6]R. M. Metcalf and D. R. Boggs, "Ethernet: Distributed packet switching for local computer networks," Communications of the ACM, 19(7):395-404, 1976.
[7]S. Molnar, J. Eyles, and J. Poulton, "Pixelflow: High-speed rendering using image composition," SIGGRAPH '92 Proceedings, pp. 231-240, 1992.
[8]L. M. Ni and P. K. McKinley, "A survey of wormhole routing techniques in direct networks," IEEE Computer, 26(2):62-76, 1993.
[9]D. Patterson, G. Gibson, and R. Katz, "A Case for redundant arrays of inexpensive disks (RAID)," Proceedings of the ACM SIGMOD Conference, pp. 109-116, June 1988.
[10]C. Ruemmler and J. Wilkes, "An introduction to disk drive modeling," IEEE Computer, 27(3):17-28, 1994.
[11]M. Smotherman, "A sequencing-based taxonomy of 1/O systems and review of historical machines," ACM Computer Architecture News, 17(5):5-15, 1989.
[12]C. Tsang, M. M. Chen, and T. Yogi. "Gigabit density magnetic recording," Proceedings of the lEEE, 83(9): 134-1359, 1993.
[13]C. Wood and P. Hodges, "DASD trends: Cost, performance, and form factor," Proceedings of the IEEE, 81(4):573-585, 1993.
