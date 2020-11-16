# RISC-V

**单指令周期 **取指、译码、执行、访存、写回

流水线执行、分支预测、乱序执行

[toc]

## ISA

软件与硬件之间的接口规范

## 模块化指令集

### RV32I

* **R 寄存器**  

  操作数rs1、rs2，

  指定接受操作结果rd，

  操作码opcode 0110011

* **I 短立即数Immediate&访存load**

  imm十二位立即数

  立即数运算和load

* **S 访存store**

  byte halfword word

* **B 条件跳转branch**

  imm位数与S相差了1位

* **U 长立即数upper imm**

  高20位立即数

  LUI AUIPC

* **J 无条件跳转jump **

  imm位数与U相差了12位

  

<img src="D:\git\Learning\RV32I.jpg">

<img src="D:\git\Learning\RV32I2.jpg">

* 通用寄存器

<img src="D:\git\Learning\RV32IR.jpg">

* **CSR 控制状态寄存器**

  有专门读写指令

**寻址**

* **立即数寻址** 操作数是指令本身的常量
* **寄存器寻址** 操作数存放在寄存器里
* **基址寻址** 基址&偏移
* **程序计数器相对寻址** B PC址&offest

**中断**

* 外部中断
* 定时器中断
* 软件中断
* 调试中断

### RV32M

<img src="D:\git\Learning\RV32M.jpg">

* **乘法** 32'bx32'b分高低32乘

### RV32F RV32D

<img src="D:\git\Learning\RV32FD.jpg">

### RV32V

<img src="D:\git\Learning\RV32V.jpg">

.vv 向量向量

.vs 向量标量

* 有32个向量寄存器，但每个向量寄存器元数个数不同，看处理器分了多少

* 最大向量长度位mvl，向量长度寄存器vl可以助于数组维度不是mvl整数倍时的编程。

* 稀疏数组，vldx vstx提供**索引**数据传输
* 每个时钟周期内操作2、4、8个64位元素
* 掩码向量vp0、vp1，第i位为1，表示元素i会被向量运算更改，为0反之