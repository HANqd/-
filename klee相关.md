以下都是参考的klee官方doxygen文档： [klee](http://www.doc.ic.ac.uk/~dsl11/klee-doxygen/index.html)

---
Doxygen：从源代码生成文档的工具。</br>
本文档对KLEE的内部运作进行了高层概述。</br>

<p>KLEE通过解释LLVM位代码来实现符号执行。 通过插入对KLEE的特殊调用（即klee_make_symbolic）来定义符号存储器，在执行过程中，KLEE会跟踪符号存储器的所有使用。 收集有关符号内存使用情况的约束。 使用先前声明的符号内存定义的内存也将变为符号。
每当遇到引用符号存储器的分支时，KLEE都会将整个状态分为两个分支，并分别探测两条分支，为此可以找到对符号约束的可能解决方案。 KLEE向STP查询以解决符号约束。</p>

本文档的其余部分描述了KLEE的一些重要组件</br>


