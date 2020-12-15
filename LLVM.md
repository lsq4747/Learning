# LLVM

LLVM的目标是生成一系列的库

因为它的设计是基于库的，它允许选择pass执行顺序

[toc]

## LLVM设计与使用

1. 交叉编译 Clang/LLVM

   在host编译并构建二进制文件，在target运行二进制文件。

2. C到LLVM汇编码

   C源码→token流（每个token可表示标识符运算符等）→语法分析器→AST树→检查语义正确性→IR

3. LLVM IR转换为bitcode

   llvm-as汇编器

4. LLVM bitcode转换为目标平台汇编码

   llc命令，把LLVM编译为特定架构的汇编语言。

   汇编文件→可执行文件，使用汇编器和连接器

5. LLVM bitcode转回为LLVM汇编码

   llvm-dis反汇编器

6. LLVM IR转换

   <img src="https://github.com/lsq4747/Learning/blob/master/llvm.JPG">

7. link LLVM bitcode

   llvm-linkM 如果一个函数或者变量在另一个文件中被引用，那么连接器就会解析这个文件中引用的符号

8. 执行bitcode

   lli工具执行bitcode格式，使用JIT执行，如果不支持JIT，用解析器

## LLVM后端

LLVM有自己定义目标机器的方式，tablegen，用tablegen来指定寄存器、指令集、调用约定。



可执行文件：llvm-tblgen

输入：.td文件

输出：.inc文件（c++）



IR→SelectionDAG （SelectionDAG中目标平台不支持的指令会被转化为合法的指令）

SelectionDAG→MachineDAG（后端指令选择）

1. 定义寄存器

   <img src="https://github.com/lsq4747/Learning/blob/master/LLVMR.JPG">

2. 定义调用约定

   调用约定，值如何传递给函数、如何从函数返回。

   返回值约定：

   <img src="https://github.com/lsq4747/Learning/blob/master/LLVMRetCC.jpg">

   参数传递约定：

   <img src="https://github.com/lsq4747/Learning/blob/master/LLVMCC.JPG">

3. 定义指令集

   指令目标描述文件定义了：操作数、汇编字符串、指令格式。<img src="https://github.com/lsq4747/Learning/blob/master/LLVMadd.jpg">

   add r0，r0，r1

4. **实现栈帧lowering**

5. 打印指令

   打印寄存器名称、打印指令

6. 选择指令

   DAG中的指令→平台对应的指令

   <kbd>xxxSelDAGToDAG.cpp</kbd>中有两个重要函数<kbd>Select()</kbd>与<kbd>SelectAddr</kbd>。

   * Select()返回一个SDNode对象，选择DAG节点的操作码
   * SelectAddr()定义地址选择函数，计算加载、存储操作的基址和偏移。选择DataDAG节点的addr类型。

7. 增加指令编码

   指令需要进行编码，可以在.td文件中指定指令的时候去指定编码

8. 子平台支持

   目标平台的变体，可能包括额外的寄存器、指令等等。

9. **多指令lowering**

10. 注册平台