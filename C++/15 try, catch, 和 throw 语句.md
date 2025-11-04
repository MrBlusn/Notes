
- `try` 块用于包含可能引发异常的代码。
- `throw` 用于抛出异常。
- `catch` 块用于捕获并处理异常。

基本语法
![[Pasted image 20251024094302.png|1500]]

**示例代码**：
```
#include <iostream>
#include <stdexcept>

// 函数，计算除法
double divide(double numerator, double denominator) {
    if (denominator == 0) {
        throw std::invalid_argument("Denominator cannot be zero."); // 抛出异常
    }
    return numerator / denominator;
}

int main() {
    double num, denom;

    std::cout << "Enter numerator: ";
    std::cin >> num;
    std::cout << "Enter denominator: ";
    std::cin >> denom;

    try {
        double result = divide(num, denom);
        std::cout << "Result: " << result << std::endl;
    } catch (std::invalid_argument &e) { // 捕获 std::invalid_argument 异常
        std::cerr << "Error: " << e.what() << std::endl;
    }

    std::cout << "Program continues after try-catch." << std::endl;
    return 0;
}
```
**预期输出**：
```
Enter numerator: 10
Enter denominator: 0
Error: Denominator cannot be zero.
Program continues after try-catch.
```


`cerr` 是 C++ 的“标准错误输出流”，用法与 `cout` 完全一样，只是目的地是 **stderr**（屏幕默认也显示，但可被重定向单独走错误通道）。  
特点：无缓冲（或立即刷新），适合快速打错误信息，防止程序崩溃时日志丢失。


### `rethrow` 异常

**描述**：可以在 `catch` 块中使用 `throw` 语句重新抛出捕获的异常，以便其他部分处理。
**示例代码**：
```
#include <iostream>
#include <stdexcept>

// 函数，抛出异常
void func1() {
    throw std::runtime_error("Error in func1.");
}

// 函数，调用 func1 并重新抛出异常
void func2() {
    try {
        func1();
    } catch (...) { // 捕获所有异常
        std::cout << "func2() caught an exception and is rethrowing it." << std::endl;
        throw; // 重新抛出当前异常
    }
}

int main() {
    try {
        func2();
    } catch (std::exception &e) { // 在 main 中捕获异常
        std::cerr << "Main caught: " << e.what() << std::endl;
    }
    return 0;
}
```
**预期输出**：
```
func2() caught an exception and is rethrowing it.
Main caught: Error in func1.
```

