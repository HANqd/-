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

