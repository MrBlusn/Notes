
在C++中，`extern` 关键字用于==声明==一个变量或函数是在另一个文件或同一个文件的其他位置定义的。这主要用于处理全局变量或函数声明，确保在多个源文件中能够正确地链接到这些全局变量或函数的定义。

==不可以实现==，只可以声名。

`#ifndef DAY05_EXTERN_GLOBAL_H`
`#define DAY05_EXTERN_GLOBAL_H`
`#include <string>`
`extern int global_age ;`
`extern std::string global_name ;`
`#endif //DAY05_EXTERN_GLOBAL_H`

