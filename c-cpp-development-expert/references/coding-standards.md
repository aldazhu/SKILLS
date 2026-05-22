# C/C++ 编码规范与最佳实践

## 目录
- [命名规范](#命名规范)
- [代码风格](#代码风格)
- [内存管理](#内存管理)
- [资源管理](#资源管理)
- [错误处理](#错误处理)
- [并发编程](#并发编程)
- [性能优化](#性能优化)
- [安全性规范](#安全性规范)

## 命名规范

### 通用规则
- **一致性**：在整个项目中保持一致的命名风格
- **可读性**：名称应清晰表达意图，避免缩写
- **长度适中**：足够描述性但不过长

### C 语言命名规范
```c
// 类型命名 - 驼峰式，大写开头
typedef struct {
    int x;
    int y;
} Point;

// 函数命名 - 下划线分隔，动词开头
int calculate_distance(Point a, Point b);
void process_data(const char* input);

// 变量命名 - 下划线分隔，小写
int user_count;
float total_amount;

// 常量命名 - 全大写，下划线分隔
#define MAX_SIZE 1024
const int DEFAULT_TIMEOUT = 30;

// 宏命名 - 全大写，下划线分隔
#define LOG_ERROR(msg) fprintf(stderr, "ERROR: %s\n", msg)

// 枚举命名 - 类型名大写开头，值全大写
typedef enum {
    Color_Red,
    Color_Green,
    Color_Blue
} Color;
```

### C++ 命名规范
```cpp
// 类/结构体命名 - 大驼峰
class NetworkServer { };
class UserData { };

// 成员变量命名 - 小驼峰，前缀下划线（可选）
class User {
public:
    std::string userName;
    int _age;  // 或使用 m_age
private:
    std::string _email;
};

// 公共成员函数命名 - 小驼峰
void sendMessage(const std::string& msg);
int getUserId() const;

// 常量命名 - 全大写
const int MAX_CONNECTIONS = 100;
constexpr double PI = 3.14159;

// 命名空间命名 - 全小写
namespace database { }
namespace utils { }

// 模板参数命名 - 单个大写字母或大驼峰
template <typename T>
class Container { };

template <typename Key, typename Value>
class HashMap { };
```

## 代码风格

### 缩进与格式
```cpp
// 使用 4 空格缩进
if (condition) {
    statement1;
    statement2;
}

// 函数定义 - 每个参数独占一行（参数多时）
void complex_function(
    int param1,
    const std::string& param2,
    double param3,
    const std::vector<int>& param4
) {
    // 实现
}

// 函数调用 - 参数多时换行
result = function_with_many_params(
    arg1,
    arg2,
    arg3,
    arg4
);

// 链式调用 - 每个操作符新行
auto result = object
    .operation1()
    .operation2()
    .operation3();
```

### 注释规范
```cpp
// 单行注释 - 用于简短说明
// 多行注释 - 用于详细说明

/**
 * @brief 函数功能简述
 * 
 * @param param1 参数1说明
 * @param param2 参数2说明
 * @return 返回值说明
 * @throws std::runtime_error 异常说明
 */
int calculate_value(int param1, double param2);

// TODO: 待办事项
// FIXME: 需要修复的问题
// HACK: 临时解决方案
// NOTE: 重要提示
```

### 头文件保护
```cpp
// 传统方式
#ifndef PROJECT_FILENAME_H
#define PROJECT_FILENAME_H

// 头文件内容

#endif // PROJECT_FILENAME_H

// 现代方式 (C++17+)
#pragma once

// 头文件内容
```

## 内存管理

### 智能指针使用
```cpp
// unique_ptr - 独占所有权
#include <memory>
std::unique_ptr<int> ptr1(new int(42));
auto ptr2 = std::make_unique<int>(42); // 推荐
ptr2 = std::move(ptr1); // 所有权转移

// shared_ptr - 共享所有权
auto shared1 = std::make_shared<int>(42);
auto shared2 = shared1; // 引用计数增加
std::cout << shared1.use_count() << std::endl; // 2

// weak_ptr - 避免循环引用
std::weak_ptr<int> weak = shared1;
if (auto locked = weak.lock()) {
    // 成功锁定，可以访问
    std::cout << *locked << std::endl;
}

// 自定义删除器
auto file_ptr = std::unique_ptr<FILE, decltype(&fclose)>(
    fopen("data.txt", "r"),
    &fclose
);
```

### 避免内存泄漏
```cpp
// 错误 - 可能内存泄漏
void bad_function() {
    int* ptr = new int(42);
    if (some_condition) {
        return; // 内存泄漏
    }
    delete ptr;
}

// 正确 - 使用智能指针
void good_function() {
    auto ptr = std::make_unique<int>(42);
    if (some_condition) {
        return; // 自动释放
    }
}

// 正确 - RAII 模式
class Resource {
public:
    Resource() {
        // 获取资源
    }
    ~Resource() {
        // 释放资源
    }
};

void use_resource() {
    Resource res; // 自动管理
}
```

## 资源管理

### RAII (Resource Acquisition Is Initialization)
```cpp
// 文件处理 - 自动关闭
#include <fstream>
void process_file() {
    std::ifstream file("data.txt");
    if (!file) {
        throw std::runtime_error("Cannot open file");
    }
    // 使用文件
    // 离开作用域时自动关闭
}

// 锁管理 - 自动释放
#include <mutex>
std::mutex mtx;

void critical_section() {
    std::lock_guard<std::mutex> lock(mtx);
    // 临界区代码
    // 离开作用域时自动解锁
}

// 使用 scoped_lock 管理多个锁
std::mutex mtx1, mtx2;
void multi_lock() {
    std::scoped_lock lock(mtx1, mtx2);
    // 临界区代码
}
```

### 容器和视图
```cpp
// 避免不必要的拷贝
void process_string(const std::string& str); // 传引用
void process_view(std::string_view sv);    // C++17 string_view

// 避免容器拷贝
void process_vector(const std::vector<int>& vec); // 传引用
void process_span(std::span<int> data);          // C++20 span

// 返回大对象
std::vector<int> generate_data() {
    return {1, 2, 3, 4, 5}; // 移动语义，无拷贝
}

// 接收移动语义
void consume_data(std::vector<int>&& vec) {
    // 可以修改 vec
}
```

## 错误处理

### 异常处理
```cpp
// 使用异常处理错误情况
#include <stdexcept>

void risky_operation() {
    if (error_condition) {
        throw std::runtime_error("Operation failed");
    }
}

// 捕获异常
try {
    risky_operation();
} catch (const std::runtime_error& e) {
    std::cerr << "Error: " << e.what() << std::endl;
} catch (...) {
    std::cerr << "Unknown error" << std::endl;
}

// noexcept 标记不抛异常的函数
void safe_function() noexcept {
    // 不抛出异常
}
```

### 错误码 vs 异常
```cpp
// 错误码方式 - 性能敏感或遗留代码
enum class ErrorCode {
    Success,
    InvalidInput,
    ResourceNotFound,
    PermissionDenied
};

ErrorCode process_with_error_code(int input, int* output) {
    if (input < 0) {
        return ErrorCode::InvalidInput;
    }
    *output = input * 2;
    return ErrorCode::Success;
}

// 异常方式 - 现代C++推荐
int process_with_exception(int input) {
    if (input < 0) {
        throw std::invalid_argument("Input must be positive");
    }
    return input * 2;
}

// std::optional - 可能失败的操作
#include <optional>
std::optional<int> safe_divide(int a, int b) {
    if (b == 0) {
        return std::nullopt;
    }
    return a / b;
}

// 使用
auto result = safe_divide(10, 0);
if (result) {
    std::cout << "Result: " << *result << std::endl;
} else {
    std::cout << "Division by zero" << std::endl;
}
```

## 并发编程

### 线程安全
```cpp
// 互斥锁保护共享数据
#include <mutex>
#include <thread>

class ThreadSafeCounter {
private:
    int counter;
    mutable std::mutex mtx;
public:
    void increment() {
        std::lock_guard<std::mutex> lock(mtx);
        ++counter;
    }
    
    int get() const {
        std::lock_guard<std::mutex> lock(mtx);
        return counter;
    }
};

// 使用原子操作避免锁
#include <atomic>
std::atomic<int> atomic_counter;

void safe_increment() {
    atomic_counter.fetch_add(1, std::memory_order_relaxed);
}

// 条件变量
#include <condition_variable>
std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void worker() {
    std::unique_lock<std::mutex> lock(mtx);
    cv.wait(lock, [] { return ready; });
    // 执行工作
}

void signal_ready() {
    {
        std::lock_guard<std::mutex> lock(mtx);
        ready = true;
    }
    cv.notify_one();
}
```

### 现代并发特性
```cpp
// std::jthread (C++20) - 自动 join
#include <thread>
void task() {
    // 工作
}
std::jthread worker(task); // 析构时自动 join

// std::latch - 等待 N 个线程完成 (C++20)
#include <latch>
std::latch completion_latch(4);
```

## 性能优化

### 移动语义
```cpp
// 避免拷贝，使用移动语义
class Data {
public:
    Data() = default;
    Data(const Data&) = delete; // 禁止拷贝
    Data(Data&&) noexcept = default; // 允许移动
};

// 传递大对象
void process_big_data(const Data& data); // 只读
void consume_big_data(Data&& data);      // 消费

// 返回优化
std::vector<int> generate_data() {
    std::vector<int> result;
    // 填充数据
    return result; // 移动语义，无拷贝
}

// 使用 emplace_back 避免临时对象
std::vector<std::string> strings;
strings.emplace_back("Hello"); // 就地构造
```

### 算法优化
```cpp
// 使用标准算法
#include <algorithm>
#include <vector>

std::vector<int> data = {3, 1, 4, 1, 5, 9, 2, 6};

// 排序
std::sort(data.begin(), data.end());

// 查找
auto it = std::find(data.begin(), data.end(), 5);
if (it != data.end()) {
    std::cout << "Found: " << *it << std::endl;
}

// 转换
std::transform(data.begin(), data.end(), data.begin(),
              [](int x) { return x * 2; });

// 并行算法 (C++17)
#include <execution>
std::sort(std::execution::par, data.begin(), data.end());
```

### 内存预分配
```cpp
std::vector<int> vec;
vec.reserve(1000); // 预分配空间，避免重复分配

// 使用 reserve 避免频繁重分配
for (int i = 0; i < 1000; ++i) {
    vec.push_back(i); // 无重分配
}
```

## 安全性规范

### 防止缓冲区溢出
```c
// C 语言安全函数
#include <stdio.h>
#include <string.h>

// 使用安全的字符串函数
char buffer[100];
snprintf(buffer, sizeof(buffer), "%s", input);  // 而非 sprintf
strncpy_s(buffer, sizeof(buffer), src, count);   // 而非 strncpy

// 边界检查
if (length < sizeof(buffer)) {
    memcpy(buffer, src, length);
}
```

### 输入验证
```cpp
// 验证输入参数
void process_input(int value) {
    if (value < 0 || value > MAX_VALUE) {
        throw std::invalid_argument("Invalid input value");
    }
    // 处理有效输入
}

// 验证字符串
bool is_valid_string(const std::string& str) {
    return !str.empty() && 
           str.size() <= MAX_LENGTH &&
           std::all_of(str.begin(), str.end(), 
                      [](char c) { return std::isalnum(c); });
}
```

### 防止整数溢出
```cpp
#include <limits>
#include <stdexcept>

int safe_add(int a, int b) {
    if (b > 0 && a > std::numeric_limits<int>::max() - b) {
        throw std::overflow_error("Integer overflow");
    }
    if (b < 0 && a < std::numeric_limits<int>::min() - b) {
        throw std::overflow_error("Integer underflow");
    }
    return a + b;
}
```

### 防止悬空指针
```cpp
// 使用智能指针
#include <memory>

std::shared_ptr<Resource> create_resource() {
    return std::make_shared<Resource>();
}

// 避免返回局部对象的指针
int* bad_function() {
    int local = 42;
    return &local; // 悬空指针
}

// 正确方式
std::unique_ptr<int> good_function() {
    return std::make_unique<int>(42);
}
```

## 最佳实践总结

### DO - 应该做的
- 使用智能指针管理动态内存
- 优先使用 `const` 和 `constexpr`
- 使用 RAII 管理资源
- 使用 `std::string_view` 和 `std::span` 避免拷贝
- 使用异常处理错误情况
- 使用标准库算法
- 保持函数简短单一职责
- 使用 `auto` 简化类型声明
- 使用命名空间组织代码
- 添加有意义的注释

### DON'T - 不应该做的
- 不要使用裸指针管理资源
- 不要忽略异常
- 不要滥用全局变量
- 不要使用魔数（使用常量）
- 不要过度优化
- 不要使用 `using namespace std;`（头文件中）
- 不要返回局部对象的引用或指针
- 不要忽略编译器警告
- 不要重复代码（DRY 原则）
- 不要过早优化
