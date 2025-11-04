
| 声明写法              | 指针变量可否重定向 | 目标对象可否被改 | 常见误称    |
| :---------------- | :-------- | :------- | :------ |
| const T * p       | ✅         | ❌        | “常量指针”❌ |
| T * const p       | ❌         | ✅        | 真正的常量指针 |
| const T * const p | ❌         | ❌        | 双const  |

指针本身是一个对象，它又可以指向另外一个对象。因此，指针本身是不是常量以及指针所指的是不是一个常量就是两个相互独立的问题

用名词==顶层==`const（top-level const）`==表示指针本身是个常量==，而用名词==底层==`const（low-level const）`表示指针==所指的对象是一个常量==。

顶层`const`可以表示任意的对象是常量，这一点对任何数据类型都适用，如算术类型、类、指针等。

底层`const`则与指针和引用等复合类型的基本类型部分有关。比较特殊的是，指针类型既可以是顶层`const`也可以是底层`const`，这一点和其他类型相比区别明显：

`int i = 0;`
`//不能改变p1的值，这是一个顶层const`
`int * const pi = &i;`
`//不能改变ci的值，这是一个顶层const`
`const int ci  = 42;`
`//允许改变p2的值，这是一个底层const`
`const int *  p2 = &ci;`
`//靠右边的const是顶层const，靠左边的const是底层const`
`const int * const p3 = p2;`
`//用于声明引用的const都是底层const`
`const int &r = ci;`

底层`const`的限制却不能忽视。当执行对象的拷贝操作时，拷入和拷出的对象==必须具有相同的底层const资格==，或者两个对象的数据类型必须能够转换。

`//指针赋值要注意关注底层const`
`//p2拥有底层const,p4无底层const，所以无法赋值`
`//int * p4 = p2;`




C++11新标准规定，允许将变量声明为`constexpr`类型以便由编译器来验证变量的值是否是一个常量表达式。声明为`constexpr`的变量一定是一个常量，而且必须用常量表达式初始化：
![[Pasted image 20251009153305.png]]

尽管不能使用普通函数作为`constexpr`变量的初始值，新标准允许定义一种特殊的`constexpr`函数。

这种函数应该足够简单以使得编译时就可以计算其结果，这样就能用`constexpr`函数去初始化`constexpr`变量了。

我们在`global.h`中定义一个`constexpr`函数

![[Pasted image 20251009153505.png]]

为了避免在多个源文件中包含同一个头文件而导致的多重定义错误，可以将 `constexpr` 函数声明为 `inline`。

`inline` 关键字允许在多个翻译单元中定义同一个函数，而不会引起链接错误。

接下来在定义一个`constexpr`变量就行了

`constexpr int sz = GetSizeConst();`



必须明确一点，在constexpr声明中如果定义了一个指针，限定符constexpr仅对指针有效，与指针所指的对象无关：
![[Pasted image 20251009154109.png]]


一个`constexpr`指针的初始值必须是`nullptr`或者0，或者是存储于某个固定地址中的对象。

函数体内定义的变量一般来说并非存放在固定地址中，因此`constexpr`指针不能指向这样的变量。

定义于所有函数体之外的对象其地址固定不变，能用来初始化`constexpr`指针

global_i是一个全局变量
![[Pasted image 20251009154149.png]]


## Const 用在函数中

`const` 写在**成员函数参数列表后面**，含义是：

> **这个成员函数承诺“我==不会修改==所属对象（*this）里的==任何数据成员==”**，  
> 因此**只能读**、不能写，否则编译器直接报错。

```cpp
struct Student {
    int id;
    void setId(int v)       { id = v; }      // 普通成员函数，可修改数据
    void show()       const { cout << id; }  // const 成员函数，只读
    void bad()        const { id = 0; }      // ❌ 编译失败：assignment of member in read-only object
};
```

“**不改变对象状态的成员函数，一律尾部加 `const`**”——  
这是 C++ 的 best practice，也是 STL 容器/算法能同时接受 `const` 与非 `const` 对象的根本原因。



| `for (const auto& student : students)` | 对容器 `students` 里的每一个元素：  <br>- `auto` 让编译器自动推导出类型 `Student`；  <br>- `const` 保证只读，不会误改学生数据；  <br>- `&` 避免拷贝，提升效率。 |
| -------------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
