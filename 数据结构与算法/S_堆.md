在 C++ 中，“堆”（Heap）是一个核心概念，主要分为两类场景：**内存管理中的堆内存**（==动态内存==）和**数据结构中的堆**（如优先队列底层的二叉堆）。

### 一、内存管理中的堆（Heap Memory）

堆内存是程序运行时由操作系统分配的动态内存区域，与栈（Stack）相对：

- 栈：自动分配 / 释放，大小有限，==存储局部变量、函数==调用上下文；
- 堆：手动分配 / 释放，大小灵活（受限于系统内存），==存储需要长期存在或动态大小的对象==。

#### 1. 堆内存的分配与释放（C++ 语法）

C++ 提供两套接口：C 风格的 `malloc/free` 和 C++ 风格的 `new/delete`（推荐）。

| 操作     | C 风格                                      | C++ 风格                   | 说明                                      |
| ------ | ----------------------------------------- | ------------------------ | --------------------------------------- |
| 分配单个对象 | `int* p = (int*)malloc(sizeof(int));`     | `int* p = new int;`      | C++ 风格会==自动调用构造函数==，C 风格仅分配内存（未初始化）     |
| 分配数组   | `int* arr = (int*)malloc(5*sizeof(int));` | `int* arr = new int[5];` | 数组版 `new[]` 分配连续内存，注意不能省略 `[]`          |
| 释放单个对象 | `free(p);`                                | `delete p;`              | C++ 风格会自动调用析构函数                         |
| 释放数组   | `free(arr);`                              | `delete[] arr;`          | ==数组必须用 `delete[]`==，否则会导致析构函数调用不全、内存泄漏 |

#### 2. 核心特性

- **手动管理**：堆内存不会像栈变量一样随作用域结束自动释放，==必须显式释放==，否则会导致**内存泄漏**（程序占用的内存无法回收）。
- **生命周期灵活**：堆对象的生命周期由程序员控制，可跨函数、跨作用域存在。
- **可能的问题**：
    - 内存泄漏：忘记释放堆内存（可通过智能指针规避）；
    - 野指针：释放后未置空，后续访问导致未定义行为；
    - 二次释放：重复释放同一块堆内存，触发程序崩溃；
    - 内存碎片：频繁分配 / 释放小块内存，导致堆内存碎片化，降低分配效率。

#### 3. 现代 C++ 解决方案：智能指针

为避免手动管理堆内存的风险，C++11 引入**智能指针**（`<memory>` 头文件），自动管理堆内存生命周期：

- `std::unique_ptr`：独占所有权，不允许拷贝，支持移动；
- `std::shared_ptr`：共享所有权，通过引用计数管理，计数为 0 时释放内存；
- `std::weak_ptr`：弱引用，不增加 `shared_ptr` 的引用计数，解决循环引用问题。
示例（智能指针替代裸指针）：
```cpp
#include <memory>
int main() {
    // 独占式管理单个堆对象
    std::unique_ptr<int> p1(new int(10));
    // 共享式管理堆数组（C++11 后支持）
    std::shared_ptr<int[]> p2(new int[5]{1,2,3,4,5});
    return 0; // 智能指针超出作用域自动释放堆内存，无泄漏
}
```


### 二、数据结构中的堆（Heap）

数据结构中的堆是一种**完全二叉树**，分为两种类型：

- 大顶堆（最大堆）：每个父节点的值 ≥ 子节点的值，堆顶是最大值；
- 小顶堆（最小堆）：每个父节点的值 ≤ 子节点的值，堆顶是最小值。

####  1. 优先队列（priority_queue）

C++ 标准库的 `priority_queue`（优先队列）是堆的封装，底层默认用 `vector` 实现大顶堆，定义在 `<queue>` 头文件：
```cpp
#include <queue>
int main() {
    // 大顶堆（默认）
    priority_queue<int> pq1;
    pq1.push(3); pq1.push(1); pq1.push(4);
    cout << pq1.top() << endl; // 4
    pq1.pop(); // 弹出堆顶
    cout << pq1.top() << endl; // 3

    // 小顶堆：指定容器和比较函数
    priority_queue<int, vector<int>, greater<int>> pq2;
    pq2.push(3); pq2.push(1); pq2.push(4);
    cout << pq2.top() << endl; // 1
    return 0;
```

完整模板定义有 3 个参数：
```cpp
template <class T, class Container = vector<T>, class Compare = less<T>>
class priority_queue;
```
每个参数的作用：

| 模板参数              | 你的取值           | 含义                                         |
| ----------------- | -------------- | ------------------------------------------ |
| `class T`         | `int`          | 优先队列中存储的元素类型（这里存 int）                      |
| `class Container` | `vector<int>`  | 底层存储数据的容器（必须是随机访问容器，默认就是 vector，也可填 deque） |
| `class Compare`   | `greater<int>` | 比较规则（决定堆的类型：大顶堆 / 小顶堆）                     |

|比较规则|含义|堆类型|堆顶元素|
|---|---|---|---|
|`less<int>`|按 “小于” 比较（默认）：`a < b` 则 a 优先级低于 b|大顶堆|队列中**最大**元素|
|`greater<int>`|按 “大于” 比较：`a > b` 则 a 优先级低于 b|小顶堆|队列中**最小**元素|
如果省略后两个参数，默认是**大顶堆**（堆顶是最大元素）

## 改变排序规则
##### 自定义类排序（按类的成员变量排序）
```cpp
// 自定义类
struct Student {
    string name;
    int score;
    Student(string n, int s) : name(n), score(s) {}
};

// 比较器：按score升序（小顶堆）
struct StudentComp {
    bool operator()(const Student& a, const Student& b) const {
        // a.score > b.score → a优先级低 → b堆顶（score最小的在堆顶）
        return a.score > b.score; 
    }
};

int main() {
    priority_queue<Student, vector<Student>, StudentComp> pq;
    pq.push(Student("Alice", 90));
    pq.push(Student("Bob", 80));
    pq.push(Student("Charlie", 85));
    
    // 堆顶是score最小的Bob
    cout << "堆顶学生：" << pq.top().name << " " << pq.top().score << endl; 
    return 0;
}
```

#### 场景 3：C++11+ 用 lambda 表达式（更灵活）

lambda 表达式无法直接作为模板参数（模板参数需是 “类型”，lambda 是 “对象”），需借助 `function` 或 `decltype` 封装：
```cpp
#include <queue>
#include <vector>
#include <functional> // 需包含此头文件
#include <iostream>
using namespace std;

int main() {
    // 定义lambda比较器：按pair的first升序（小顶堆）
    auto cmp = [](const pair<int, int>& a, const pair<int, int>& b) {
        return a.first > b.first; // a.first大 → a优先级低 → b堆顶
    };

    // 方式：用decltype获取lambda类型，同时将lambda作为构造参数传入
    priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pq(cmp);
    
    pq.push({3, 1});
    pq.push({1, 3});
    pq.push({2, 2});
    
    // 堆顶是first最小的{1,3}
    cout << "堆顶：(" << pq.top().first << "," << pq.top().second << ")" << endl; 
    return 0;
}
```