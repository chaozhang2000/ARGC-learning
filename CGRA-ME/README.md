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


