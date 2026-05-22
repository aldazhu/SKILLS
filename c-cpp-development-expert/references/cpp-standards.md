# C/C++ 语言标准特性详解

## 目录
- [C 语言标准](#c-语言标准)
- [C++ 语言标准](#c-语言标准-1)
- [版本迁移指南](#版本迁移指南)
- [常用特性对比](#常用特性对比)

## C 语言标准

### C99 特性
- **变量声明**：支持在代码块任意位置声明变量
- **内联函数**：`inline` 关键字支持函数内联
- **变长数组**：运行时确定大小的数组 `int n = 10; int arr[n];`
- **复数类型**：`complex.h` 提供复数支持
- **布尔类型**：`stdbool.h` 提供 `bool`、`true`、`false`
- **指定初始化**：结构体成员按名称初始化
- **预定义宏**：`__func__` 获取当前函数名

**示例**：
```c
// 变量声明
for (int i = 0; i < 10; i++) { /* ... */ }

// 指定初始化
struct Point { int x, y; };
struct Point p = { .x = 10, .y = 20 };

// 变长数组
void process(int n) {
    int data[n]; // 运行时确定大小
}
```

### C11 特性
- **线程支持**：`threads.h` 提供线程创建和管理
- **原子操作**：`stdatomic.h` 提供原子类型
- **匿名结构体**：支持匿名结构体和联合体
- **泛型选择**：`_Generic` 宏实现类型选择
- **静态断言**：`_Static_assert` 编译时断言
- **对齐支持**：`alignof`、`_Alignas`

**示例**：
```c
// 泛型选择
#define cbrt(X) _Generic((X), \
    long double: cbrtl, \
    default: cbrt, \
    float: cbrtf \
)(X)

// 静态断言
_Static_assert(sizeof(int) == 4, "int must be 4 bytes");

// 匿名结构体
struct { int x, y; } point;
point.x = 10; // 直接访问成员

// 原子操作
#include <stdatomic.h>
atomic_int counter;
atomic_fetch_add(&counter, 1);
```

### C17 特性
- **C11 修正**：修正 C11 中的问题，无重大新特性
- **删除某些过时特性**：如 `gets()` 函数

## C++ 语言标准

### C++11 核心特性
- **智能指针**：`std::unique_ptr`、`std::shared_ptr`、`std::weak_ptr`
- **移动语义**：右值引用 `T&&` 和 `std::move`
- **Lambda 表达式**：匿名函数支持 `[captures](params) -> return_type { body }`
- **自动类型推导**：`auto` 关键字
- **范围 for 循环**：`for (auto& item : container)`
- ** nullptr**：空指针常量，替代 NULL
- ** constexpr**：编译期常量表达式
- **类型别名**：`using` 别名语法
- **tuple**：`std::tuple` 泛型元组
- **正则表达式**：`<regex>` 库

**示例**：
```cpp
// 智能指针
#include <memory>
std::unique_ptr<int> ptr(new int(42));
std::shared_ptr<int> shared = std::make_shared<int>(42);

// 移动语义
std::string str1 = "Hello";
std::string str2 = std::move(str1); // str1 被置空

// Lambda
auto add = [](int a, int b) { return a + b; };
int result = add(3, 4); // 7

// auto 类型推导
auto x = 42; // int
auto y = 3.14; // double

// 范围 for
std::vector<int> nums = {1, 2, 3, 4, 5};
for (auto& num : nums) {
    num *= 2;
}

// nullptr
void func(int* ptr);
func(nullptr); // 替代 NULL

// constexpr
constexpr int square(int x) { return x * x; }
constexpr int value = square(5); // 编译期计算
```

### C++14 特性
- **泛型 Lambda**：`auto` 参数支持
- **函数返回类型推导**：`auto` 作为返回类型
- **变量模板**：模板变量支持
- **二进制字面量**：`0b101010`
- **数字分隔符**：`1'000'000`
- **make_unique**：`std::make_unique` 工厂函数
- **shared_timed_mutex**：共享互斥锁

**示例**：
```cpp
// 泛型 Lambda
auto add = [](auto a, auto b) { return a + b; };
add(3, 4); // int
add(3.5, 2.1); // double

// 函数返回类型推导
auto get_value() { return 42; }

// 二进制字面量
int binary = 0b101010; // 42

// 数字分隔符
int million = 1'000'000;
double pi = 3.14159'26535;

// make_unique
#include <memory>
auto ptr = std::make_unique<int>(42);
```

### C++17 特性
- **结构化绑定**：`auto [x, y] = pair;`
- **if constexpr**：编译期条件
- **折叠表达式**：参数包展开
- **inline 变量**：内联变量定义
- **std::filesystem**：文件系统库
- **std::optional**：可能为空的值
- **std::variant**：类型安全的联合体
- **std::any**：类型擦除的值容器
- **并行算法**：执行策略参数

**示例**：
```cpp
// 结构化绑定
std::pair<int, int> p = {1, 2};
auto [x, y] = p; // x=1, y=2

std::map<int, std::string> m = {{1, "one"}};
for (auto& [key, value] : m) {
    std::cout << key << ": " << value << std::endl;
}

// if constexpr
template <typename T>
auto get_value(T t) {
    if constexpr (std::is_pointer_v<T>)
        return *t;
    else
        return t;
}

// 折叠表达式
template <typename... Args>
auto sum(Args... args) {
    return (args + ... + 0);
}

// inline 变量
inline int global_counter = 0;

// std::optional
#include <optional>
std::optional<int> try_parse(std::string str) {
    if (str == "42")
        return 42;
    return std::nullopt;
}

// std::variant
std::variant<int, std::string> var;
var = 42;
var = "hello";

// std::filesystem
#include <filesystem>
namespace fs = std::filesystem;
fs::path p = "/tmp/test.txt";
if (fs::exists(p)) {
    std::cout << "File size: " << fs::file_size(p) << std::endl;
}
```

### C++20 特性
- **Concepts**：概念约束模板参数
- **Ranges**：范围库和管道操作
- **Coroutines**：协程支持 `co_await`、`co_yield`、`co_return`
- **Modules**：模块系统替代头文件
- **三向比较**：`<=>` 运算符
- **std::format**：类型安全的字符串格式化
- **std::span**：连续序列视图
- **std::jthread**：自动 join 的线程
- **consteval**：立即求值函数
- **contains**：容器包含检查

**示例**：
```cpp
// Concepts
template <typename T>
concept Integral = std::is_integral_v<T>;

template <Integral T>
T add(T a, T b) {
    return a + b;
}

// Ranges
#include <ranges>
std::vector<int> nums = {1, 2, 3, 4, 5};
auto filtered = nums | std::views::filter([](int x) { return x % 2 == 0; });
auto transformed = filtered | std::views::transform([](int x) { return x * 2; });

// Coroutines
#include <coroutine>
struct Task {
    struct promise_type {
        Task get_return_object() { return {}; }
        std::suspend_never initial_suspend() { return {}; }
        std::suspend_never final_suspend() noexcept { return {}; }
        void return_void() {}
        void unhandled_exception() { std::terminate(); }
    };
};

Task async_function() {
    co_await std::suspend_always{}; // 挂起
    co_return; // 返回
}

// 三向比较
struct Point {
    int x, y;
    auto operator<=>(const Point&) const = default;
};

// std::format
#include <format>
std::string msg = std::format("Hello {}, you are {} years old", "Alice", 30);

// std::span
#include <span>
void process(std::span<int> data) {
    for (int x : data) {
        std::cout << x << " ";
    }
}

// std::jthread
#include <thread>
std::jthread worker([] {
    while (true) {
        // 工作循环
    }
}); // 自动 join

// consteval
consteval int square(int x) { return x * x; }
constexpr int result = square(5); // 必须在编译期求值

// contains
std::vector<int> v = {1, 2, 3};
if (v.contains(2)) {
    std::cout << "Found 2" << std::endl;
}
```

### C++23 特性
- **std::expected**：表示可能失败的操作
- **Deducing this**：显式对象参数
- **标准库模块**：标准库模块化支持
- **视图扩展**：更多 ranges 视图
- **print**：简化的输出函数

**示例**：
```cpp
// std::expected (C++23)
#include <expected>
std::expected<int, std::string> divide(int a, int b) {
    if (b == 0)
        return std::unexpected("Division by zero");
    return a / b;
}

// 使用
auto result = divide(10, 2);
if (result) {
    std::cout << "Result: " << *result << std::endl;
} else {
    std::cout << "Error: " << result.error() << std::endl;
}

// print (C++23)
#include <print>
std::print("Hello, {}!", "World");
```

## 版本迁移指南

### 从 C++98/03 到现代 C++
**优先替换的内容**：
1. 裸指针 → `std::unique_ptr` / `std::shared_ptr`
2. `NULL` → `nullptr`
3. 手动内存管理（`new`/`delete`）→ 智能指针 + RAII
4. `std::auto_ptr` → `std::unique_ptr`
5. `boost::shared_ptr` → `std::shared_ptr`
6. 旧式遍历 → 范围 for 循环
7. 函数指针 → Lambda 表达式

**逐步迁移策略**：
```cpp
// 旧代码
void old_style() {
    int* arr = new int[100];
    for (int i = 0; i < 100; i++) {
        arr[i] = i * 2;
    }
    delete[] arr;
}

// 现代代码
void modern_style() {
    std::vector<int> arr(100);
    for (auto& item : arr) {
        item = item * 2;
    }
}

// 旧代码 - 资源管理
void process_file() {
    FILE* f = fopen("data.txt", "r");
    if (f) {
        // 处理文件
        fclose(f);
    }
}

// 现代代码 - RAII
#include <fstream>
void process_file() {
    std::ifstream f("data.txt");
    // 处理文件，自动关闭
}
```

## 常用特性对比

### 智能指针使用场景
| 场景 | 推荐类型 | 说明 |
|------|---------|------|
| 独占所有权 | `std::unique_ptr` | 不可复制，只能移动 |
| 共享所有权 | `std::shared_ptr` | 引用计数，可复制 |
| 循环引用 | `std::weak_ptr` | 观察者，不增加引用计数 |

### 容器选择建议
| 需求 | 推荐容器 | 时间复杂度 |
|------|---------|-----------|
| 快速查找 | `std::unordered_map` | O(1) 平均 |
| 有序查找 | `std::map` | O(log n) |
| 随机访问 | `std::vector` | O(1) |
| 头部插入 | `std::deque` | O(1) |
| 中间插入 | `std::list` | O(1) |

### 最佳实践
- 优先使用 `auto` 简化类型声明，但保持可读性
- 使用 `std::make_unique` 和 `std::make_shared` 创建智能指针
- 范围 for 循环优先于传统索引遍历
- Lambda 优先于函数对象和函数指针
- `std::optional` 替代指针表示可能为空的值
- `std::string_view` 避免字符串拷贝
- `std::span` 避免容器拷贝
- 使用 `[[nodiscard]]` 标记必须检查返回值的函数
