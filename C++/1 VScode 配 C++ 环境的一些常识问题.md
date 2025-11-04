
C++ 是一种==编译==语言，这意味着程序的源代码必须经过翻译（编译）才能在计算机上运行。C/C++ 扩展不包含 C++ 编译器或调试器，因为 VS Code 作为编辑器依赖于开发工作流程的命令行工具。

某些平台上已预装的常见编译器包括Linux 上的[GNU 编译器集合](https://wikipedia.org/wiki/GNU_Compiler_Collection)(GCC) 和 macOS 上带有[Xcode 的](https://developer.apple.com/xcode/)[Clang](https://wikipedia.org/wiki/Clang)工具。

MSVC（Microsoft Visual C++）和 MinGW（Minimalist GNU for Windows）是两种主流的 C/C++ 编译工具链

编译工具链（**Toolchain**）是**一整套开发工具的集合**，用于将**源代码**转换为**可执行程序**或**库文件**。它不仅仅是“编译器”，而是一个**完整的工作流程系统**，涵盖从代码到最终产物的所有环节。

**编译工具链 = 编译器 + 链接器 + 汇编器 + 标准库 + 调试器 + 辅助工具 + 目标平台规范**

|工具/组件|作用说明|
|:--|:--|
|**编译器**|将源代码（`.c/.cpp`）编译为汇编代码（`.s`）或目标文件（`.o/.obj`）  <br>如：`gcc`, `clang`, `cl.exe`|
|**汇编器**|将汇编代码转换为机器码（目标文件）  <br>如：`as`, `ml.exe`|
|**链接器**|将多个目标文件和库链接成最终可执行文件（`.exe`, `.dll`, `.so`）  <br>如：`ld`, `link.exe`|
|**标准库**|提供 C/C++ 标准函数实现（如 `printf`, `std::vector`）  <br>如：`libc`, `libstdc++`, `MSVCRT`|
|**调试器**|用于调试运行中的程序（断点、堆栈跟踪）  <br>如：`gdb`, `lldb`, `vsdebug`|
|**辅助工具**|如：`make`, `cmake`, `ar`（打包静态库）、`objdump`（反汇编）、`strip`（去符号）|
|**目标平台规范**|定义二进制接口（ABI）、调用约定、目标架构（如 x86_64-w64-mingw32）|
