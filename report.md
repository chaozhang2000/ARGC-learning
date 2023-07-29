# CGRA(Coarse-Grained Reconfigurable Arrays)调研

**目标:** CGRA在Zynq的PL端进行实现，设计一个计算加速系统，并运行一些算法程序进行验证。  

## 架构与框架

|名称   |时间   |机构   |会议   |备注|
|:---:  |:---:  |:---:  |:---:  |:---:  |
|ADRES  |2003   |比利时鲁汶大学 |FPL    |架构   |
|CGRA-ME  |2017   |多伦多大学|ASAP(Application-Specific Systems, Architectures, and Processors)    |框架 不可商用|
|CGRA-Flow|2020   |Cheng Tan |ICCD(International Conference on Computer Design)    |框架 开源 pymtl3|
|Pillars  |2020   |北大高能效计算与应用中心 |WOSET(Workshop on Open-Source EDA Technology)    |框架 开源 Chisel|
|REVAMP |2022   |新加坡国立大学 LSP课题组 |ASPLOS    |框架 异质CGRA|
|DSAGEN |2020   |加州大学洛杉矶分校 PolyArch课题组|ISCA    |框架 开源 Chisel|
|UPTRA  |2023   |复旦大学王伶俐 |FCCM    |框架 未开源 Chisel|

备注：架构是只提出了硬件实现的想法，框架是至少完成了硬件的建模和进行映射的方法;  
Chisel和pymtl3指用来对硬件建模所使用的语言

1. 整体上大同小异，一个自家的硬件模型一个Mapper成一个框架，但在FPGA上部署的很少，我猜测是因为CGRA本来的目的就是要做成芯片的，部署在FPGA上面积和频率应该不理想，而且要考虑不少实现的细节。
2. 是有前景的，CGRA-ME针对FPGA进行优化可以跑到300M很惊人，而且和RISC-V处理器结合设计SOC对一些算法实现10倍以上加速。
3. 在上述框架中，我最倾向于CGRA-ME的设计，这个多伦多大学的研究团队从2017年提出他们的CGRA-ME框架以来，研究持续到2022年都持续有顶会论文产出且脉络清晰。在提出框架后的5年时间内先后实现了硬件的优化如在FPGA上的部署优化，Mapper算法的优化，加速系统的搭建(处理器+CGRA)，到考虑多线程的支持。现在他们已经出现商用的趋势，所以在有框架之后，进行加速系统的搭建和多线程的探索是合理的。
4. 对框架的选择问题，选定谭诚老师的CGRA-Flow，主要是学习一个框架所需要的时间成本很高，谭老师方便交流。但现在的CGRA-Flow要在FPGA上实现不是直接可用的
    * 还没完全支持数据流驱动执行，每周期每个Tile需要一条指令，浪费很多存储空间。
    * 生成的硬件全是单周期的，资源占用和频率的问题。
    * 当前硬件只支持整数操作。
    * 对立即数的处理存在问题，目前是提前将每个PE需要用到的立即数写死在一个存储器中，且Mapper没有自动从llvm汇编中获取立即数的功能，需要手动获取，如果程序变复杂这会非常繁琐。
    * 目前采用的是将数据从内存存入CGRA的数据存储器中，CGRA从数据存储器中取数计算再存回到数据寄存器中，CGRA计算完后将数据从数据存储器再重新复制到内存中。在这个过程中，要面对如何管理CGRA数据存储器问题。在CGRA-ME中，不存在CGRA内的数据存储器，CGRA直接和处理器共享内存所以不存在这个问题。

## 计划
