# CGRA-learning

## CGRA-框架的选择

### CGRA-Flow

[CGRA-Flow](https://github.com/tancheng/CGRA-Flow)  
该框架是完全开源的，主要组成部分是VectorCGRA用来生成CGRA硬件，并进行CGRA硬件的仿真。CGRA-Mapper则是配套的CGRA编译器。  

**对此CGRA框架的调研的初步结果是:**  
1. 用清晰的CGRA结构和CGRA-Mapper帮助理解CGRA的工作原理，并且可以作为模拟器进行CGRA和Mapper算法的研究。
2. 目前的建模是在几乎所有操作都单周期完成的基础上进行的，不太合理
3. 目前的操作划分还不够合理，如整数的加法和浮点数的加法没有分开这不太合理。
4. 目前的架构对存储器过于理想化，每个Tile都可以访问统一存储器，多端口存储器太过于理想化。而且还要与控制处理器共享内存。
5. 对于带有立即数操作的指令，需要提前将立即数放入常数队列中，如何获取这些常数较困难，部署时如何放入常数队列是问题。

CGRA硬件和CGRA-Mapper是高度耦合的，上面的一些问题都同时牵扯到硬件和Mapper。

**IDEA:**
1. 建模的时候应该考虑每个操作多周期的可能性,还有不定周期的可能性比如load和store，这样即使加上特殊的FU也可以轻松应对。
2. 每个时钟周期一条指令太多了，导致指令数量太多需要想办法减少指令？ 我的想法是不用时钟来驱动指令的更换，而用数据流来驱动，这样空指令不用再放在指令存储器中存储。
3. 将立即数的传递交给指令来做，指令中加入立即数？
4. 访存的问题需要解决，共享存储器的话。 设计一个多端口的Cache然后接到内存上？
5. 对于OpenCGRA采用的语言需要选择，最好是能生成芯片级别RTL的语言，一步到位，例如Chisel

### Pillars 

来自北大，硬件部分用Chisel编写，上了FPGA板，效果不佳跑不到100M,估计实现得比较拉跨   
[论文 1  Pillars: An Integrated CGRA Design Framework](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/Pillars.pdf)  

### garnet

斯坦福大学的框架，还没找到提出框架的论文，TODO

### CGRA-ME

多伦多大学的框架，感觉是来真的，准备好好看一下  
看到第一篇相关论文  
[2 CGRA-ME: An Open-Source Framework for CGRA Architecture and CAD Research (ASAP invited Paper )](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/CGRA-ME_An_Open-Source_Framework_for_CGRA_Architecture_and_CAD_Research__Invited_Paper.pdf)  
**特点:**  
1. 代码开源但不可商用。
2. 上板了，且针对FPGA进行了优化，能跑到300M，非常惊人。
3. 与RISC-V处理器共同设计了一个SOC，能够对计算密集型的算法进行高一个数量级的加速也非常惊人,很振奋。
4. 做了上下文的设计(这里还没有细看)
5. 设计了官网很正式[官网](https://cgra-me.ece.utoronto.ca)

准备先看一下CGRA-ME相关的论文[论文列表](https://cgra-me.ece.utoronto.ca/pubs/)  
首先看最早的一篇论文

#### [3 CGRA-ME: A Unified Framework for CGRA Modelling and Exploration (2017 ASAP)](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/asap2017.pdf)

会议全名：International Conference on Application-specific Systems, Architectures, and Processors，本文是和CGRA框架相关的工作。
* RELATED WORK
	* 作者对CGRA硬件和Mapper以及其他相关工作比如模拟器做了一些介绍**TODO:**其中一些参考文献可能值得看。 其中对CGRA硬件之间的不同之处做出了归纳:1.包含的类似ALU的计算块2.连接方式3.内存，在CGRA内还是外部还是内外部混合4.动态可重配置性和上下文相关。
* CGRA-ME FRAMEWORK OVERVIEW
	* 基本思路和OpenCGRA相同，这项工作定制了一个CGRA描述语言，所有的扩展功能从这个描述开始。另外作者还有建立systemC模型的计划，这里也可以借鉴，C模型可以快速验证Mapper的正确性。
* Mapper
	* 其Mapper也是采用MRRG(Modulo Routing Resource Graph)(**TODO**这里有一篇参考文献关于什么)逐周期考虑资源的使用进行Mapping，但这里作者提到了这样Mapping与多上下文架构很重要，所以多上下文架构的支持可能对Mapper有要求？**TODO**。 The MRRG representation is especially useful for multiple-context architectures where routes and functional units can be shared to perform different operations on sub- sequent cycles in a repeating pattern.
* ARCHITECTURE EXPLORATION CASE STUDY 
	* 提到了存储器相关的设计，A single memory-access unit that can perform loads and stores is dedicated to each row within all architecture variants. The outputs of each block within a row is connected to the inputs of the access unit (address and data-in) while the output of the memory block (data-out) is connected to each of the blocks in the row as an additional input.应该和下图类似，一行共享一个访存单元。**其具体如何工作还不知道**，**但我认为把访存独立出来是合理的，因为访存存在时钟周期不确定的问题。独立出来可能可以方便Mapping**。
* Standard-Cell Implementation Results
	* 最初的CGRA4x4 65nm工艺的结果是200MHz，17年到现在FPGA上已经优化到300MHz
<p float="center">
	<img src="https://github.com/chaozhang2000/CGRA-learning/blob/main/pic/mem.png" width="400" />
</p>

#### [4 Architecture Exploration of Standard-Cell and FPGA-Overlay CGRAs Using the Open-Source CGRA-ME Framework (ISPD 2018)](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/ispd003i-chinA.pdf)

这个会议是集成电路的顶会之一。
这篇文章感觉创新点没有很明显，但参考文献24有一种CGRA硬件的架构ADRES不是第一次见了，看这篇文章14

#### [5 An Architecture-Agnostic Integer Linear Programming Approach to CGRA Mapping (DAC 2018)](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/dac2018.pdf)

这篇文章是EDA的顶会，可以和Mapping算法相关，看摘要是一种在"一系列"硬件架构下都适用的Mapping方法，我主要关注硬件跳过。

#### [6 Compact Area and Performance Modelling for CGRA Architecture Evaluation (FPT 2018)](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/Compact_Area_and_Performance_Modelling_for_CGRA_Architecture_Evaluation.pdf)

FPGA领域的顶会，本篇文章的内容和CGRA的面积与性能建模相关，跳过

#### [7 Generic Connectivity-Based CGRA Mapping via Integer Linear Programming (FCCM 2019)](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/Generic_Connectivity-Based_CGRA_Mapping_via_Integer_Linear_Programming.pdf)

FPGA领域的顶会，但本篇文章的内容和Mapping算法的加速有关

#### [8 Impact of FPGA Architecture on Area and Performance of CGRA Overlays(FCCM 2019)](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/Impact_of_FPGA_Architecture_on_Area_and_Performance_of_CGRA_Overlays.pdf)

FPGA领域的顶会，这篇文章是和在FPGA部署相关的，考虑底层FPGA架构对CGRA面积和性能的影响，对CGRA覆盖层进行优化，实现资源占用的减少

#### 9 Double-Pumping the Interconnect for Area Reduction in Coarse-Grained Reconfigurable Arrays (ASAP 2021)

这篇没什么意思

#### [10 Power, Performance and Area Consequences of Multi-Context Support in CGRAs (ASAP 2021)](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/Power_Performance_and_Area_Consequences_of_Multi-Context_Support_in_CGRAs.pdf)

这篇文章介绍CGRA中的多上下文支持的实现方法，可以实现CGRA的分时复用，就像操作系统中的多任务。

#### 11 An Open-Source Framework for the Generation of RISC-V Processor + CGRA Accelerator Systems

对CGRA的应用，但这篇文章暂时还没找到，与RISC-V处理器构成加速器系统

#### 12 CGRA Mapping Using Zero-Suppressed Binary Decision Diagrams (ASP-DAC)
#### 13 Elastic Multi-Context CGRAs 

总结：  
* 这个研究团队从2017年发布CGRA相关成果至今，一直有持续性的产出。
* 文章主要发表在计算机体系结构和FPGA两个领域的顶会上，ASAP和FCCM比较多
* 工作开展的流程：
	* 框架的搭建包括硬件和Mapper
	* 硬件的优化(面积的优化和针对FPGA的结构对FPGA上部署的优化) Mapper算法的优化(加速)
	* 加速系统的搭建，处理器+CGRA
	* 上下文支持的探索
* 在其研究开展的初期，框架总体发了一篇体系结构，Mapping算法发了一篇EDA，面积和性能分析发了一篇FPGA

#### [14 ADRES: An Architecture with Tightly Coupled VLIW Processor and Coarse-Grained Reconﬁgurable Matrix (2003)](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/mei2003.pdf)

对这篇文章的兴趣来自前面的论文4。这篇文章发表自2003年，主要是描述了一种CGRA的架构，是一种**紧耦合的CGRA**。所谓的紧耦合指CGRA和处理器共享内存。作者分析这样做会给编程带来便利，不用为了将数据从CPU侧的存储器复制到CGRA侧的处理器进行大量编程。这种架构的框图大致如下图所示。
<p float="center">
	<img src="https://github.com/chaozhang2000/CGRA-learning/blob/main/pic/mem1.png" width="400" />
</p>
在Conslusions and Future Work中作者提到他们知识提出这样的框架但是还没有做电路上的实现，所以准备看看有没有相关的实现。

## DSAGEN

这个研究团队看起来更强，发的全是体系结构的顶会，来自加州大学洛杉矶分校  
[教授Tony Nowatzki主页](http://web.cs.ucla.edu/~tjn/)  
[PloyArch research group主页](https://polyarch.cs.ucla.edu/)  
[DSAGEN github](https://github.com/PolyArch/dsa-framework)  
[MICRO会议上关于DSAGEN的教学视频](https://www.youtube.com/watch?v=ghqr69qfPmY)  
