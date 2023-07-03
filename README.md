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
1. 建模的时候应该考虑每个操作多周期的可能性，这样即使加上特殊的FU也可以轻松应对。
2. 每个时钟周期一条指令太多了，导致指令数量太多需要想办法减少指令？ 我的想法是不用时钟来驱动指令的更换，而用数据流来驱动，这样空指令不用再放在指令存储器中存储。
3. 将立即数的传递交给指令来做，指令中加入立即数？
4. 访存的问题需要解决，共享存储器的话。 设计一个多端口的Cache然后接到内存上？
5. 对于OpenCGRA采用的语言需要选择，最好是能生成芯片级别RTL的语言，一步到位，例如Chisel

### Pillars 

来自北大，硬件部分用Chisel编写，上了FPGA板，效果不佳跑不到100M,估计实现得比较拉跨   
[论文 Pillars: An Integrated CGRA Design Framework](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/Pillars.pdf)  

### garnet

斯坦福大学的框架，还没找到提出框架的论文，TODO

### CGRA-ME

多伦多大学的框架，感觉是来真的，准备好好看一下  
看到第一篇相关论文  
[CGRA-ME: An Open-Source Framework for CGRA Architecture and CAD Research (ASAP invited Paper )](https://github.com/chaozhang2000/CGRA-learning/blob/main/paper/CGRA-ME_An_Open-Source_Framework_for_CGRA_Architecture_and_CAD_Research__Invited_Paper.pdf)
**特点:**  
1. 代码开源但不可商用。
2. 上板了，且针对FPGA进行了优化，能跑到300M，非常惊人。
3. 与RISC-V处理器共同设计了一个SOC，能够对计算密集型的算法进行高一个数量级的加速也非常惊人,很振奋。
4. 做了上下文的设计(这里还没有细看)
5. 设计了官网很正式[官网](https://cgra-me.ece.utoronto.ca)

准备先看一下CGRA-ME相关的论文
