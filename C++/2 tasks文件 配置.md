![[Pasted image 20251007153112.png]]


```json
{
    "tasks": [ ... ],     // 一个任务列表，可以放多个编译任务
    "version": "2.0.0"    // 配置文件格式版本号，固定写法
}
```

```json
"type": "cppbuild"
```
- 表示这是一个 **C++ 构建任务**，VS Code 会用它来做语法高亮、错误跳转等。

```json
"label": "C/C++: g++.exe 生成活动文件"
```
- 任务的 **显示名称**，你在 VS Code 的“终端”→“运行任务”里看到的就是这个。

```json
"command": "C:/msys64/ucrt64/bin/g++.exe"
```
- 实际调用的 **编译器命令**，这里是 MSYS2 的 UCRT64 环境下的 g++。

```json
"args": [
    "-fdiagnostics-color=always",
    "-g",
    "${file}",
    "-o",
    "${fileDirname}\\${fileBasenameNoExtension}.exe"
]
```
- 传给 g++ 的 **参数列表**，逐条解释：

| 参数                                               | 含义                             |
| :----------------------------------------------- | :----------------------------- |
| `-fdiagnostics-color=always`                     | 让编译器输出带颜色的错误信息，更好看             |
| `-g`                                             | 生成调试信息，方便你用 GDB 调试             |
| `${file}`                                        | 当前打开的 `.cpp` 文件完整路径            |
| `-o`                                             | 指定输出文件名                        |
| `${fileDirname}\\${fileBasenameNoExtension}.exe` | 输出路径：和源文件同目录，名字相同，扩展名改为 `.exe` |

```json
"options": {
    "cwd": "C:/msys64/ucrt64/bin"
}
```
- **工作目录**，也就是运行 g++ 时所在的目录，这里是 g++ 所在的目录，避免找不到依赖。

```json
"problemMatcher": ["$gcc"]
```
- 告诉 VS Code **如何解析编译器输出**，以便在“问题”面板里正确显示错误和警告行号。

```json
"group": {
    "kind": "build",
    "isDefault": true
}
```
- 把这个任务归为 **“构建”类任务**，并设为 **默认构建任务**。

```json
"detail": "调试器生成的任务。"
```
- 任务的 **描述文字**，鼠标悬停在任务列表里能看到，影响不大。



`gcc` 和 `g++` 是 **GNU 编译器集合（GNU Compiler Collection）** 中的两个命令，它们都可以编译 C/C++ 代码，但行为有区别。

|命令|编译语言|默认链接标准库|编译 `.cpp` 文件时|
|:--|:--|:--|:--|
|`gcc`|**C**|只链 **C 标准库**|不会自动链 **libstdc++**（C++ 标准库）|
|`g++`|**C++**|自动链 **libstdc++**|把 `.cpp` 当 C++ 文件处理|
用 `g++` 编译 C++ 程序最省事；`gcc` 更适合纯 C 程序，编译 C++ 时要手动加 `-lstdc++`，不如直接用 `g++`。

