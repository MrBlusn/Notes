# 序列容器
## vector

## stack

## queue


## priority_queue
### 一、priority_queue 核心原理

`priority_queue` 是 C++ STL 中的**优先队列**容器适配器，底层默认基于 `vector` 实现，内部通过**堆排序**维护一个 “堆结构”（默认是**大顶堆**）：
- 核心操作（`push/pop/top`）的时间复杂度均为 O (logn)（堆的调整成本）。

### 二、priority_queue 基本用法

#### 1. 头文件

```cpp
#include <queue> // 必须包含，priority_queue定义在此
```

#### 2. 定义方式（重点）

|类型|定义代码|说明|
|---|---|---|
|默认大顶堆（int）|`priority_queue<int> pq;`|堆顶是最大元素|
|小顶堆（int）|`priority_queue<int, vector<int>, greater<int>> pq;`|堆顶是最小元素|
|自定义类型（如 pair）|`priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;`|按 pair 默认规则（先 first 后 second）小顶堆|

#### 3. 核心操作

|操作|代码示例|说明|
|---|---|---|
|入队|`pq.push(5);`|将元素插入堆，自动调整堆结构|
|取堆顶元素|`pq.top();`|返回堆顶元素（大顶堆返回最大，小顶堆返回最小），不删除|
|出队（删堆顶）|`pq.pop();`|删除堆顶元素，自动调整堆结构|
|判空|`pq.empty();`|空返回 true，否则 false|
|获取大小|`pq.size();`|返回队列中元素个数|

## list





# 关联容器

## map
`map` 是 C++ STL 中**有序键值对（key-value）容器**，底层基于**红黑树**（自平衡二叉搜索树）实现，核心特性：

- **键（key）唯一**且**自动按升序排列**（可自定义排序规则）；
- **值（value）可重复**，可通过键快速访问对应值；
- 增删查的时间复杂度稳定为 O (logn)（红黑树特性）；
- 本质是 “关联容器”，通过键映射值，而非下标（虽支持 `[]` 语法，但底层是查找 / 插入）。

### 一、核心头文件
```cpp
#include <map>
```
---
### 二、基本用法

#### 1. 定义与初始化
```cpp
#include <map>
#include <iostream>
using namespace std;

int main() {
    // 1. 空map（默认按key升序）
    map<int, string> m1;

    // 2. 初始化列表（key自动排序，去重）
    map<int, string> m2 = {{3, "three"}, {1, "one"}, {2, "two"}, {1, "重复key"}}; 
    // 最终存储：{1:"one", 2:"two", 3:"three"}（key唯一，重复key被覆盖）

    // 3. 拷贝构造
    map<int, string> m3(m2);

    // 4. 自定义排序（按key降序）
    map<int, string, greater<int>> m4 = {{3, "three"}, {1, "one"}}; 
    // 存储：{3:"three", 1:"one"}

    return 0;
}
```

#### 2. 核心操作（增 / 删 / 查 / 遍历）

|操作|代码示例|说明|
|---|---|---|
|**插入键值对**|`m.insert({1, "one"});`|插入，key 重复则忽略（返回 `pair<迭代器, bool>`，bool 为 false）|
|**下标访问 / 插入**|`m[2] = "two";`|若 key 不存在则插入（值为默认构造），存在则修改对应值|
|**查找 key**|`m.find(3);`|找到返回迭代器（`it->first`=key，`it->second`=value），否则返回 `end()`|
|**判断 key 存在**|`m.count(2);`|存在返回 1，否则 0（仅判断存在性）|
|**删除 key**|`m.erase(2);`|删除 key 为 2 的键值对，返回删除个数（0 或 1）|
|**删除迭代器指向**|`m.erase(m.find(1));`|高效删除，无需查找（迭代器有效时）|
|**范围删除**|`m.erase(m.lower_bound(2), m.end());`|删除 key≥2 的所有键值对|
|**清空 map**|`m.clear();`|清空所有键值对|
|**获取大小 / 判空**|`m.size(); m.empty();`|元素个数；空返回 true|
|**边界查询**|`m.lower_bound(2);`|返回 key≥2 的第一个键值对迭代器|
|**边界查询**|`m.upper_bound(2);`|返回 key>2 的第一个键值对迭代器|
### 三、核心特性与注意事项

#### 1. `[]` 运算符的特殊行为（重点）

`map` 的 `[]` 不是普通 “下标访问”，而是**查找 + 插入**：

- 若 key 存在：返回对应 value 的引用（可修改，如 `m[1] = "new";`）；
- 若 key 不存在：自动插入该 key，value 为**默认构造值**（如 `string` 为空串，`int` 为 0）。
 ```cpp
    map<int, int> m;
    cout << m[5]; // key=5不存在，插入{m[5]=0}，输出0
    cout << m.size(); // 1
    ```

**若仅需查询 key 是否存在，避免用`[]`**，优先用 `find()` 或 `count()`（不会插入无效 key）。

#### 2. 键的不可修改性

`map` 的 key 是只读的（红黑树结构依赖 key 的有序性），若需修改 key，需先删除旧键值对，再插入新的：
```cpp
map<int, string> m = {{1, "one"}};
// m.find(1)->first = 2; // 编译错误：key不可修改
m.erase(1);
m.insert({2, "one"}); // 正确
```

#### 3. 支持重复键的容器：`multimap`

- `map` 不允许 key 重复，若需一个 key 对应多个 value，改用 `multimap`；
- `multimap` 无 `[]` 运算符（一个 key 对应多个值，无法下标访问）；
- 用 `equal_range(key)` 获取该 key 对应的所有值的迭代器范围：

  ```cpp
    multimap<int, string> mm = {{1, "one"}, {1, "壹"}, {2, "two"}};
    auto [first, last] = mm.equal_range(1);
    for (auto i = first; i != last; ++i) {
        cout << i->second << " "; // 输出：one 壹
    }    ```


**map 中的每一个 “键值对元素” 本质上就是一个 `pair<const Key, T>` 类型的对象**——==`pair` 是 map 存储单个键值对的 “载体”==，但并非 map 专属的 “子项”（`pair` 是 C++ 通用的模板类型，可独立使用）。



## set