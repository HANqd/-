# Source Overview
This section gives a brief overview of how KLEE’s source code is structured:

- include/ Contains the publicly exported header files. That is header files that are accessible throughout the source code.
- tools/ Has the main functions for all KLEE binaries in the bin/ directory. Note that some are Python scripts.
- lib/ Contains most of the code.
- lib/Core Contains code that interprets and executes the LLVM bitcode and KLEE’s memory model. The Executor.cpp class is usually a good starting point for any KLEE extension.
- lib/Expr has KLEE’s expression library.
- lib/Solver contains all the solvers (STP, Z3, MetaSMT) as well as all the solvers in the solver chain (Independent Solver and Counterexample Cache).
- lib/Module deals with manipulating the LLVM bitcode before it is executed. It links in the (POSIX) runtime functions, runs optimisations and other passes. Some of these put the LLVM bitcode in the state KLEE expects and insert some runtime checks (such as instrumenting the division operations to check for div by zero errors).
- runtime/ contains the various runtime KLEE supports. That is code that is linked in with the program KLEE analyses before execution.
- tests/ contains small C programs and LLVM bitcode that is used as a regression test suite for KLEE.

# main.c分析
（1）程序刚开始用#include引入了一些头文件，这里不再细看。</br>
（2）接下来声明命名空间，使用namespace。为的是把全局作用域划分为几个部分，不同命名空间中的标志符可以同名而不发生冲突。在命名空间里声明了几个选项，分别如下：</br>
- 测试用例选项：这些选项选择文件来生成每个测试用例。</br>
- 启动选项：这些选项影响程序执行的启动方式。</br>
- 链接选项：这些选项控制链接的库。</br>
- 检查选项：这些选项控制KLEE进行的某些检查。</br>
- 重置选项：这些选项影响测试用例的重置。</br>

在上述的选项中，代码中详细的介绍了选项下边的设置，这里不再展开。</br>
（3）定义了一个KleeHandler类，包含下边代码所用到的成员函数，包括测试用例的数目，探测到的路径数目，设置解释器等函数。</br>
