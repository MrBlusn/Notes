
## vector删除特定元素

正确姿势：使用 Erase-Remove 惯用法。
```cpp
#include <iostream>
#include <vector>
#include <algorithm>   // std::remove

int main() {
    std::vector<int> v = {1, 2, 3, 2, 4, 2, 5};

    int target = 2;

    // 1. remove 把“不等于 target”的元素前移，返回新逻辑结尾
    // 2. erase 把尾部多余元素真正删掉
    v.erase(std::remove(v.begin(), v.end(), target), v.end());

    for (int x : v) std::cout << x << ' ';   // 1 3 4 5
}
```

remove：
**“逻辑”删除**——把**不需要的元素全部搬到尾部**，并返回**新的逻辑末尾迭代器**；**容器大小不变**。
```cpp
std::vector<int> v{1,2,3,2,4};
auto new_end = std::remove(v.begin(), v.end(), 2);
// 遍历打印看现状
for (int x : v) std::cout << x << ' ';   // 1 3 4 4 4
std::cout << "\nsize=" << v.size();      // size=5  （没改）
```
物理布局：`1 3 4 | 4 4`（竖线后是被“垃圾”覆盖的旧数据），==此时 remove 返回的迭代器是指向竖线后的第一个元素的==


erase:
`vector::erase` 有两个重载：
```cpp
iterator erase(const_iterator pos);                 // 删一个
iterator erase(const_iterator first, const_iterator last); // 删区间
```

`v.erase(it)` 返回的就是：
**一个迭代器，指向“被删元素的下一个位置”**。

`erase(const_iterator first, const_iterator last)` 同样返回一个 **iterator**，指向 **"被删除区间的下一个位置"**。
删除 `[first, last) 区间内的所有元素` （注意是==左闭右开==）
如果 last == end()，则返回 end()。


```cpp
for (auto it = v.begin(); it != v.end(); )     // 注意没有 ++it
{
    if (/* 某条件，例如 *it % 2 == 0 */)
        it = v.erase(it);   // 删除偶数，并拿到下一个位置
    else
        ++it;               // 只有没删才手动后移
}
```
**一旦执行了 `it = v.erase(it);`**，`erase` 已经把 `it` 更新成**下一个位置**，**如果再写一次 `++it`**，就会**跳过 1 个元素**，造成漏检查甚至越界。






### 1.  引言
向量（**Vector**）是 C++ 标准模板库（STL）中的一种序列容器，能够==动态==地管理==可变大小==的数组。与传统的固定大小的数组不同，向量可以根据需要随时调整其大小，提供更高的灵活性和便利性。

### 2. `std::vector` 基础
使用 `std::vector` 需要包含 `<vector>` 头文件：`#include <vector>`
![[Pasted image 20251010153403.png|475]]

#### 2.3 向量的大小与容量

- `size()`：返回向量中元素的数量。

- `capacity()`：返回向量目前为止分配的存储容量。

- `empty()`：检查向量是否为空。

### 3. 向量的基本操作

#### 3.1 添加与删除元素

- `push_back()`：在向量末尾添加一个元素。

- `pop_back()`：移除向量末尾的元素。

- `insert()`：在指定位置插入元素。

- `erase()`：移除指定位置的元素或范围内的元素。 

- `clear()`：移除所有元素。
![[Pasted image 20251010153513.png|625]]

#### 3.2 访问元素

- `operator[]`：通过索引访问元素。

- `at()`：通过索引访问元素，带==边界检查==。

- `front()`：访问第一个元素。

- `back()`：访问最后一个元素。
![[Pasted image 20251010153628.png|525]]

#### 3.3 遍历向量

- **使用范围** `for` **循环**

- **使用传统** `for` **循环**

- **使用迭代器**
![[Pasted image 20251010154239.png|500]]

#### 3.4 修改元素

- **通过索引或迭代器修改**

- **使用** `assign()` **重新赋值**

- **替换整个向量内容**
![[Pasted image 20251010154441.png|475]]

### 4. 向量的高级用法
#### 4.1 嵌套向量（二维向量）

向量可以包含其他向量，形成多维数组结构。

![[Pasted image 20251010154636.png|500]]

#### 4.2 向量与其他数据结构结合

向量可以与结构体、类等其他数据结构结合使用，增强数据组织能力。
![[Pasted image 20251010154704.png|500]]

#### 4.3 使用迭代器操作向量

迭代器是一种指针类型，用于遍历和操作容器中的元素。
![[Pasted image 20251010154739.png|450]]

### 5. 常用算法与向量
#### 5.1 排序

可以使用 `<algorithm>` 头文件中的 `sort()` 函数对向量进行排序。只要告诉它“按什么规则比大小”。
## 最简例子：按成绩升序
```cpp
#include <algorithm>   // std::sort
#include <vector>
using namespace std;

// 比较函数：成绩小的排前面
bool cmpByGrade(const Student& a, const Student& b) {
    return a.grade < b.grade;
}

int main() {
    vector<Student> stu = { {3,"Bob",85}, {1,"Ann",92}, {2,"Tom",78} };

    sort(stu.begin(), stu.end(), cmpByGrade);   // 排序

    for (const auto& s : stu) s.printInfo();    // 78 85 92
}
```

## C++11 以后：lambda 更简洁
```cpp
// 按成绩降序
sort(stu.begin(), stu.end(),
     [](const Student& a, const Student& b){
         return a.grade > b.grade;   // 大于号就是降序
     });
```
## 按学号 / 姓名字典序 同理
```cpp
sort(stu.begin(), stu.end(),
     [](const Student& a, const Student& b){
         return a.id < b.id;        // 按学号升序
     });
```

#### 5.2 反转

使用 `reverse()` 函数可以反转向量中的元素顺序。

注意区分两个层次的“reverse”：

1. `<algorithm>` 里的 `std::reverse`  
    把**整个容器**或**一段区间**就地倒序，用法：
   ```cpp
    #include <algorithm>
    std::reverse(v.begin(), v.end());   // 向量 v 首尾互换
    ```
2. 容器自己提供的 `vector::reverse_iterator`（或 `rbegin()`/`rend()`）  
    只是**反向遍历**，并不改变容器内元素的真实顺序。


```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main(){
    vector<int> v{1,2,3,4,5};

    reverse(v.begin(), v.end());   // 就地反转

    for(int x:v) cout << x << ' '; // 5 4 3 2 1
}
```
## 常见误区

- `reverse(v)` // ❌ 编译失败，必须给两个迭代器参数。
- 想“排序后再反转”可以两句连写：
```cpp
    sort(v.begin(), v.end());      // 先升序
    reverse(v.begin(), v.end());   // 再整体倒置 → 降序
    ```


### 6. 向量的性能与优化

#### 6.1 内存管理
向量会动态地管理内存，自动调整其容量以适应新增或删除的元素。频繁的内存分配可能会影响性能。

#### 6.2 预留空间
使用 `reserve()` 可以提前为向量分配足够的内存，减少内存重新分配的次数，提高性能。
`reserve()` 只干一件事：**一次性把底层容量（capacity）涨到“至少 n”，但元素个数（size）保持为 0**。  
换句话说——**只买地，不盖房**；后续 `push_back`/`emplace_back` 如果不超过这块地，就不会触发重新分配、拷贝、释放，效率自然高。

## 常见误区

1. `reserve()` **不会改变 size**，下标访问要先 `push_back`/`emplace_back` 或用 `resize()`。
 ```cpp
    v.reserve(100);
    cout << v.size();     // 0
    v[0] = 10;            // ❌ UB，越界
    ```
    
2. 与 `resize()` 区别
    
    - `reserve(n)` → 容量 ≥ n，size 不变，元素不存在。
        
    - `resize(n)` → 容量 ≥ n，size 变成 n，多出元素默认构造。
        
3. 重复调用 `reserve()` 如果新值 ≤ 原容量，**什么也不会发生**（标准保证不收缩）。

#### 6.3 收缩容量
使用 `shrink_to_fit()` 可以请求收缩向量的容量以匹配其大小，释放多余的内存。





