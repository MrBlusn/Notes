在 C++ 编程中，处理字符串和数字之间的转换是一项常见的任务。
`sstream` 是 C++ 标准库中的一个组件，它提供了一种方便的方式来处理字符串流（可以像处理流一样处理字符串）。
`<sstream>` 允许你将字符串当作输入/输出流来使用，这使得从字符串中读取数据或将数据写入字符串变得非常简单。

`sstream`是 C++ 标准库中的一个命名空间，它包含了几个类，用于处理字符串流，这些类包括：
- `istringstream`：用于从字符串中读取数据。
- `ostringstream`：用于将数据写入字符串。
- `stringstream`：是`istringstream`和`ostringstream`的组合，可以同时进行读取和写入操作。


### 从字符串读取数据
下面是一个使用 `istringstream` 从字符串中读取整数和浮点数的例子：
![[Pasted image 20251015154016.png]]
**输出结果：**
![[Pasted image 20251015154128.png]]

### 向字符串写入数据
下面是一个使用 `ostringstream` 将数据写入字符串的例子：
![[Pasted image 20251015154157.png]]
**输出结果：**
![[Pasted image 20251015154214.png]]


### 使用stringstream进行读写操作
下面是一个使用 `stringstream` 同时进行读取和写入操作的例子：
![[Pasted image 20251015154255.png]]
**输出结果：**
![[Pasted image 20251015154311.png]]

## **stringsteam 的用途**
- 利用 stringstream 去除字符串空格：`stringstream` 默认是以空格来分割字符串的，利用 **`stringstream`** 去除字符串空格非常方便![[Pasted image 20251015154835.png]]
- 利用 stringstream 指定字符分割字符串：![[Pasted image 20251015154801.png]]
- 类型转换：使用 **`stringstream`** 将数字转换为字符串；反过来，也可以将字符串转换为数值类型。