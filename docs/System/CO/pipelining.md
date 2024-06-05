---
comments: true
---

# Pipelining

流水线 CPU 设计。

## 1 Why pipelining

单周期 CPU 的问题：

* 时钟周期需要根据最长的指令来确定。例如 load 指令：instruction memory -> register file -> ALU -> data mamory -> register file. 这经过了很长的时间。
* 也即对于不同的指令来说都需要这么长的 period，违反了 "Make the common case fast" 原则。

为了解决这种问题，我们引入**流水线设计**。

这里给出一个流水线的具象化描述：

![20240526224238](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240526224238.png)

需要注意的是，流水线对于单个指令的延迟时间来说**并没有提升**，它的作用是提高程序（多个指令）的吞吐率（improving of throughput），进而提高用到的元件的工作效率。

## 2 Pipelining in RISC-V

在 RISC-V 架构中，流水线具有以下五个**阶段（stages）**：

1. IF: Instruction fetch from momory
2. ID: Instruction decode & register read
3. EX: Execute operation or calculate address
4. MEM: Access data memory operand
5. WB: Write result back to register

事实上，RISC-V 架构本身是对流水线较为友好的，原因有：

* 所有的指令都是 32 位，这样在一个 cycle 中更容易进行 fetch 和 decode；
* 指令格式较少且较为常规，这样解码和读寄存器都可以在一步中进行；
* 读写内存只存在于 load 和 store 当中，这样可以在第三阶段进行计算地址，第四阶段访问内存。

**ISA 的设计会影响流水线设计的复杂度**。

## 3 Pipelined Datapath

由于流水线程序执行结构的特殊性，在数据通路里面我们需要在每两个阶段中间加入寄存器用来存储前一个阶段所产生的数据或信号，如下图：

![20240527154100](https://cdn.jsdelivr.net/gh/Frankoxer/image-host/pic/20240527154100.png)

需要注意的是这些 latch 并不是越多越好，因为 latch 本身也有开销，包括空间上的和时间上的。

因此流水线会有以下的一些关于性能的问题：

* Latency。相较于单周期，流水线的每一个指令的执行时间实际上**更长了**。
* Imbalance。阶段之间的不平衡会影响性能。
* Overhead rise from register delay and clock skew also contribute to the lower limit of machine cycle. 寄存器延迟和时钟偏差也会有影响。
* Hazard。竞争冒险是影响流水线性能的大问题。
* Time to “fill” pipeline and time to “drain” it reduces speedup.