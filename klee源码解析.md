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
（3）定义了一个KleeHandler类，包含下边代码所用到的成员函数，包括测试用例的数目，探测到的路径数目，设置解释器等函数。接着分别定义了每个成员函数，这些函数如下：</br>

- Kleehandler：构造函数，初始化一些成员变量。创建输出目录（OutputDir or "klee-out-\<i\>"），打开warning.txt、messages.txt、info文件。
- ~KleeHandler：析构函数，释放一些需要释放的成员变量。
- getInfoStream：得到输入文件信息流。
- getNumTestCases：得到目前为止成功产生的测试用例数目。
- getNumPathsExplored：得到目前为止探测到的路径数目。
- incPathsExplored：探测路径。
- setInterpreter：设置解释器。分别有setPathWriter、setSymbolicPathWriter。
- processTestCase：输出描述一个测试用例的所有文件（.ktest,.kquery,.cov,等）
- getOutputFilename：得到输出文件的路径。其中把路径和文件名添加到系统环境变量中。
- openOutputFile：打开输出文件。
- getTestFilename：得到测试文件名。
- openTestFile：打开测试文件。
- loadPathFile：加载路径文件。
- getKTestFilesInDir：得到Ktest文件的路径。
- getRunTimeLibraryPath：得到运行时库的路径


