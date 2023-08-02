# CGRA-ME (CGRA-Modelling and Exploration)

[官网](https://cgra-me.ece.utoronto.ca/) https://cgra-me.ece.utoronto.ca/  
多伦多大学 [Jason Anderson教授](https://janders.eecg.utoronto.ca/) https://janders.eecg.utoronto.ca/  
**背景** Xilinx首席工程师和经理，在Xilinx工作十年，研究和开发综合工具。  
**研究兴趣** 计算机硬件设计特别是可编程逻辑器件，FPGA和CGRA，目前对CGRA的应用感兴趣特别是在嵌入式系统和功率受限的环境中。  
**工作** 1.HLS LegUp 2020年被微芯收购。 2.CGRA-ME

## [CGRA-ME: A Unified Framework for CGRA Modelling and Exploration (2017 ASAP)](https://github.com/chaozhang2000/ARGC-learning/blob/main/paper/asap2017.pdf) 

CGRA-ME的第一篇论文，提出CGRA-ME框架
* 介绍部分
	* 指出了CGRA的优势，包括improved speed,area-efficiency and power relative to FPGAs.
	* 指出了提出CGRA-ME框架的目的，在作者看来没有类似的框架对CGRA进行科学探索，作者认为他这个是第一个框架
	* 指出CGRA-ME是什么，包括CGRA架构描述，建模，应用程序的编译，系统仿真和物理实现的框架。
* 主体部分
	* CGRA-ME大致的工作流程是可以用一种CGRA的描述语言描述一个CGRA硬件，这个表述会被翻译成一个Device Model和一个RTL Model,Device Model用于Mapper,RTL的模型可以用于综合。
	* 主体部分的小章节包括**LLVM FRONT-END**(CGRA-Flow和它一样) **Mapper**(和CGRA-Flow思路差不多，也是基于MRRG,也就说明所有操作的周期确定，采用的是模拟退火算法)
	* 对于其生成的CGRA硬件的PE见图2,PE连接见图6,最关心的访存设计有提到，每一行有一个专门的访存单元，每行的每个PE都有一个输出连接到访存单元，访存单元则有一个输出连接到此行的每个PE
* 实验部分
	* 用17个Benchmark分别在CGRA-ME生成的4x4,5x5,6x6大小的，两种连接方式，但上下文和双上下文的情况下进行Map，给出了是否可以成功Map的结果证明框架可以对CGRA进行科学探索。
	* 在65nm的工艺库下对4x4的CGRA给出了面积和延时数据,200M左右的频率

框架的提出，只是介绍了框架包含的内容，实验只是验证了框架的可用性，即可以Map，生成的硬件是可以综合的，但是离SOC应该还有不少距离。

## [Architecture Exploration of Standard-Cell and FPGA-Overlay CGRAs Using the Open-Source CGRA-ME Framework (ISPD 2018)](https://github.com/chaozhang2000/ARGC-learning/blob/main/paper/ispd003i-chinA.pdf)

ISPD(International Symposium on Physical Design 国际物理设计研讨会)  
这篇文章和2017年提出框架的文章大致相同，很多的内容是在重复  
主要补充了CGRA-ME生成的CGRA的物理实现部分，用CGRA-ME生成4x4的ADRES架构的CGRA，并对其分别进行了集成电路(Standard-Cell)和FPGA(FPGA-Overlay)的物理实现，给出了一些面积频率的数据，虽然在17年的文章中也有一些Standard-Cell物理实现的内容，但本篇文章中内容更加丰富，且添加了FPGA-Overlay的内容。主要数据是FreePDK45 standard-cell library下频率150M左右，Altera Stratix IV FPGA上部署80M~100M左右。  
其工作所谓的创新点在于，之前的框架工作没有物理实现部分，之前物理实现的部分也是脱离框架的，他们做了一个结合。


## [An Architecture-Agnostic Integer Linear Programming Approach to CGRA Mapping (DAC 2018)](https://github.com/chaozhang2000/ARGC-learning/blob/main/paper/dac2018.pdf)

这篇文章主要时Mapper算法的实现，暂时不太关注，主要是说提出了一种Mapper算法，并与模拟退火算法进行比较，发现其映射的效果更好，算是在Mapper算法上的探索。


## [Compact Area and Performance Modelling for CGRA Architecture Evaluation (FPT 2018)](https://github.com/chaozhang2000/ARGC-learning/blob/main/paper/Compact_Area_and_Performance_Modelling_for_CGRA_Architecture_Evaluation.pdf)

这篇文章是指在CGRA-ME中添加了面积和时序评估的内容，也是对CGRA-ME框架的改进，但是对我们参考价值不大

## [Generic Connectivity-Based CGRA Mapping via Integer Linear Programming (FCCM 2019)](https://github.com/chaozhang2000/ARGC-learning/blob/main/paper/Generic_Connectivity-Based_CGRA_Mapping_via_Integer_Linear_Programming.pdf)

这篇文章主要是对其Mapper算法(ILP)进行改进,实现更快的速度，先不关注Mapper部分

## [Impact of FPGA Architecture on Area and Performance of CGRA Overlays(FCCM 2019)](https://github.com/chaozhang2000/ARGC-learning/blob/main/paper/Impact_of_FPGA_Architecture_on_Area_and_Performance_of_CGRA_Overlays.pdf)

这篇文章是对CGRA在FPGA上部署的优化，具体的方法还没细看

## [An Open-Source Framework for the Generation of RISC-V Processor CGRA Accelerator Systems](https://github.com/chaozhang2000/ARGC-learning/blob/main/paper/An_Open-Source_Framework_for_the_Generation_of_RISC-V_Processor__CGRA_Accelerator_Systems.pdf)

这篇文章是比较需要关注的一篇文章，这个团队搭建了一个RISC-V+CGRA的加速系统，向应用又靠了一步。
* CGRA硬件: 经典的ADRES架构，32位宽互连。有对每个PE的描述，比**OpenCGRA要简洁**
* RISC-V核: PULP project中一个RISC-V核的删减版，32IM
* 系统架构: 见论文图3,系统主要由处理器存储器和CGRA组成，处理器与CGRA的交互主要由CGRA的IO和两者的共享内存进行，采用哈佛架构，处理器有一个独立的指令存储器，另外一个共享内存可以被CGRA和Processor同时访问，且被CGRA访问的时候是被分成4块分别被每一行访问。
* 控制: 每个操作的时钟周期都是确定的，通过一个时钟计数器来确定CGRA是否已经工作完成。

**他们的共享内存用的是一个片上的存储器也就是一个verilog描述的模型，显然这是一个理想的情况，数据在片上就能存储下太过理想，这种理想化的存储器模型也带来了访存所需时钟周期确定的结果。所以这个系统实际上只能在仿真情况下运行。**

* 编程模型与案例研究: 作者给出了一个矩阵乘法的案例，但是在描述的时候，**作者描述了矩阵的数据需要被提前按照一定的规定手动放入数据存储器中，这是一个非常严格的要求，缺少自动化。且作者给出的矩阵乘法的数据流图过于漂亮，很难相信可以通过程序自动生成这么漂亮的数据流图**
* 主要实验: 
	* 结果:将矩阵乘法FIR等四种算法在系统中运行，用4x4的CGRA达到11-19倍加速，8x8达到14-37倍加速,cpu和CGRA采用相同的时钟频率。
	* 结果分析: 加速的原因1.pipeline parallelism 好到像手动设置的映射结果，II=1。2.CGRA的计算是无分支的只有一个计数器来标志CGRA工作的结束，但是处理器则需要使用比较和分支来检测循环结束这是需要时间的。3.充分发掘出了访存的并行性。
* 其他实验:作者还对设计进行了综合可能是为了满足投稿的需求，然后还实现了一种架构的CGRA进行了类似实验，证明框架的灵活性。

**评价:** 好像靠近应用了，但好像又没有，有较多理想的假设，比如巨大的多端口的片上存储，比如数据提前在存储器中正确排列，比如并行性极高的数据流图。
**IDEA** 但实际上这篇文章给我不少启发。
1. 一个CGRA有一个适当容量的DataMemory,需要在CGRA开始计算之前由处理器将数据按照一定的规则从外部存储器中存入这个DataMemory中，这一部分的数据可以支持一部分时间的计算，例如完整循环中的前几次计算，计算结束后，处理器将输出数据从DataMemory中取出，再重新导入新的数据继续计算，来解决需要巨大片上存储器的问题。
2. 无论是理想容量的片上处理器还是上面一点的想法中，都需要对数据在CGRA的DataMemory中进行重新的排布，这个排布的规则应该由mapper来产生，然后cpu可以根据这个规则来提前向CGRA的DataMemory中加入数据，我并没有发现有文章描述了这个问题，我认为这可以称为是对数据的Map，算是一个创新点。
3. 我认为CGRA的大小应该是有讲究的，存储器的端口数量肯定不能太大，所以一个CGRA的大小应该有限，至少能访存的Tile应该有限，其他Tile太多可能并没有用，反而用多个小的CGRA组成阵列可能有意义，毕竟在2中我们认为可以将循环的执行在时间上进行拆分，那么在空间上拆分应该也可以。
4. CGRA硬件有值得学习的地方。

# 计划
1. CGRA Generator 用Chisel来编写，结合CGRA-ME和CGRA-Flow的优点，相比CGRA-Flow修改ALU为固定两输入一输出，取消Crossbar，改为三个MUX进行简化，保留configMem，Constqueue，和控制流支持的部分。
2. CGRA和处理器的接口实现，实现利用Zynq中的ARM对CGRA的配置和对CGRA中DataMemory的填充。
3. 实现CGRA-Mapper自动生成常数->实现数据的Map
4. 在ARM中编写软件，实现对CGRA的配置和控制,运行一些例如矩阵乘法和FIR验证正确性和分析加速效果。
