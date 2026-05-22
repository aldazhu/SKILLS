# C/C++ 版本特性与兼容性查询

## 目录
- [版本概览](#版本概览)
- [C 语言版本特性](#c-语言版本特性)
- [C++ 版本特性](#c-版本特性)
- [特性查询方法](#特性查询方法)
- [编译器支持情况](#编译器支持情况)
- [版本迁移指南](#版本迁移指南)
- [跨平台兼容性](#跨平台兼容性)

## 版本概览

### C 语言标准时间线

| 版本 | 发布年份 | 别名 | 主要特性 |
|------|---------|------|---------|
| C89/C90 | 1989/1990 | ANSI C | 首个标准 |
| C99 | 1999 | - | 变长数组、内联函数、复数、bool |
| C11 | 2011 | - | 线程、原子操作、匿名结构体 |
| C17 | 2017 | - | C11 技术修正 |
| C23 | 2023 | (草案) | 位操作库、增强特性 |

### C++ 标准时间线

| 版本 | 发布年份 | 别名 | 主要特性 |
|------|---------|------|---------|
| C++98 | 1998 | - | 首个标准 |
| C++03 | 2003 | - | 技术修正 |
| C++11 | 2011 | C++0x | 智能指针、Lambda、移动语义、auto |
| C++14 | 2014 | C++1y | 泛型 Lambda、make_unique |
| C++17 | 2017 | C++1z | filesystem、optional、variant |
| C++20 | 2020 | C++2a | concepts、ranges、modules、coroutines |
| C++23 | 2023 | C++2b | std::expected、print、Deducing this |

## C 语言版本特性

### C99 新增特性

#### 核心语言特性
```c
// 1. 变量声明位置（可在块内任意位置）
for (int i = 0; i < 10; i++) {
    int temp = i * 2;
}

// 2. 指定初始化器
struct Point { int x, y; };
struct Point p = { .y = 10, .x = 20 };

// 3. 变长数组（VLA）
void func(int n) {
    int arr[n];  // 运行时确定大小
}

// 4. 单行注释
// 这是单行注释

// 5. restrict 关键字
void copy(int *restrict dest, const int *restrict src, size_t n);

// 6. 内联函数
static inline int square(int x) { return x * x; }

// 7. 复数类型
#include <complex.h>
double complex z = 3.0 + 4.0 * I;

// 8. bool 类型
#include <stdbool.h>
bool flag = true;

// 9. __func__ 预定义标识符
void my_function() {
    printf("Function: %s\n", __func__);  // 输出 "my_function"
}

// 10. 变参宏
#define DEBUG(fmt, ...) printf(fmt, ##__VA_ARGS__)
```

#### 标准库新增
```c
// <stdbool.h> - bool 类型
// <complex.h> - 复数运算
// <tgmath.h> - 类型泛型数学
// <inttypes.h> - 整数格式化
// <stdint.h> - 标准整数类型（int32_t, uint64_t 等）
// <wchar.h> - 宽字符
```

**版本标识**：`__STDC_VERSION__` >= 199901L

### C11 新增特性

#### 核心语言特性
```c
// 1. 匿名结构体和联合体
struct {
    struct { int x, y; };  // 匿名结构体
    int id;
} point;
point.x = 10;  // 直接访问成员

// 2. _Generic 泛型选择
#define cbrt(X) _Generic((X), \
    long double: cbrtl, \
    default: cbrt, \
    float: cbrtf \
)(X)

// 3. _Static_assert 编译期断言
_Static_assert(sizeof(int) == 4, "int must be 4 bytes");

// 4. _Noreturn 函数属性
_Noreturn void fatal_error(void) {
    abort();
}

// 5. 对齐支持
_Alignas(16) int aligned_int;
size_t alignment = _Alignof(int);  // 类似 C++ 的 alignof
```

#### 标准库新增
```c
// <threads.h> - 线程支持
#include <threads.h>
thrd_t thread;
thrd_create(&thread, thread_func, arg);

// <stdatomic.h> - 原子操作
#include <stdatomic.h>
atomic_int counter;
atomic_fetch_add(&counter, 1);

// <stdnoreturn.h> - noreturn 宏
#include <stdnoreturn.h>
noreturn void exit_now(void);

// <uchar.h> - UTF-16/32 字符
char16_t c16 = u'中';
char32_t c32 = U'中';

// <time.h> - 新增时间函数
timespec_get(&ts, TIME_UTC);
```

**版本标识**：`__STDC_VERSION__` >= 201112L

### C17 特性
- C17 主要是 C11 的技术修正
- 没有新增语言特性
- 修正了一些缺陷报告

**版本标识**：`__STDC_VERSION__` >= 201710L

### C23 新增特性（草案）

#### 预期新增特性
```c
// 1. 位操作库
#include <stdbit.h>
unsigned int leading_zeros = stdc_leading_zeros_ui(32);

// 2. 增强的 bool 类型
#include <stdbool.h>
bool true_val = true;  // 更严格的 bool 支持

// 3. 属性语法增强
[[nodiscard]] int check_status(void);

// 4. 二进制字面量
int binary = 0b101010;  // 类似 C++14

// 5. 数字分隔符
int million = 1'000'000;  // 类似 C++14
```

**版本标识**：`__STDC_VERSION__` >= 202311L（草案值）

## C++ 版本特性

### C++11 新增特性

#### 核心语言特性
```cpp
// 1. 类型推导
auto x = 42;  // int
auto y = 3.14;  // double
decltype(x) z = x;  // int

// 2. 范围 for 循环
std::vector<int> vec = {1, 2, 3, 4, 5};
for (auto& item : vec) {
    item *= 2;
}

// 3. Lambda 表达式
auto add = [](int a, int b) { return a + b; };
int result = add(3, 4);  // 7

// 4. 智能指针
std::unique_ptr<int> ptr(new int(42));
std::shared_ptr<int> shared = std::make_shared<int>(42);

// 5. 移动语义
std::string str1 = "Hello";
std::string str2 = std::move(str1);  // str1 被置空

// 6. nullptr
void func(int* ptr);
func(nullptr);  // 替代 NULL

// 7. 右值引用
void process(std::string&& str);  // 接受右值

// 8. noexcept
void safe_func() noexcept;  // 声明不抛异常

// 9. constexpr
constexpr int square(int x) { return x * x; }
constexpr int val = square(5);  // 编译期计算

// 10. 类型别名
using String = std::string;  // 替代 typedef
template <typename T>
using Vec = std::vector<T>;

// 11. 初始化列表
std::vector<int> vec = {1, 2, 3, 4, 5};

// 12. 委托构造函数
class MyClass {
public:
    MyClass(int x) : x_(x) {}
    MyClass() : MyClass(0) {}  // 委托
};

// 13. 继承构造函数
class Derived : public Base {
public:
    using Base::Base;  // 继承构造函数
};

// 14. override 和 final
class Base {
public:
    virtual void func() {}
    virtual void final_func() final {}
};

class Derived : public Base {
public:
    void func() override {}  // 明确覆盖
    // void final_func() {}  // 编译错误：final 函数不能覆盖
};

// 15. 禁用默认函数
class NoCopy {
public:
    NoCopy(const NoCopy&) = delete;  // 禁止拷贝
    NoCopy& operator=(const NoCopy&) = delete;
};
```

#### 标准库新增
```cpp
// <thread> - 线程支持
#include <thread>
std::thread t([] { /* work */ });
t.join();

// <mutex> - 互斥锁
#include <mutex>
std::mutex mtx;
std::lock_guard<std::mutex> lock(mtx);

// <condition_variable> - 条件变量
#include <condition_variable>
std::condition_variable cv;

// <atomic> - 原子操作
#include <atomic>
std::atomic<int> counter(0);

// <future> - Future 和 Promise
#include <future>
std::future<int> fut = std::async([] { return 42; });
int result = fut.get();

// <chrono> - 时间库
#include <chrono>
auto now = std::chrono::system_clock::now();

// <regex> - 正则表达式
#include <regex>
std::regex pattern(R"(\d+)");
std::smatch match;

// <array> - 固定大小数组
#include <array>
std::array<int, 5> arr = {1, 2, 3, 4, 5};

// <unordered_map>, <unordered_set> - 哈希容器
#include <unordered_map>
std::unordered_map<int, std::string> umap;

// <tuple> - 元组
#include <tuple>
auto t = std::make_tuple(1, "hello", 3.14);

// <memory> - 智能指针
#include <memory>
std::unique_ptr<int> uptr = std::make_unique<int>(42);
std::shared_ptr<int> sptr = std::make_shared<int>(42);

// <functional> - 函数对象
#include <functional>
std::function<int(int)> f = [](int x) { return x * 2; };

// <type_traits> - 类型萃取
#include <type_traits>
static_assert(std::is_integral<int>::value);

// <random> - 随机数生成器
#include <random>
std::mt19937 gen(std::random_device{}());
std::uniform_int_distribution<> dis(1, 100);
```

**版本标识**：`__cplusplus` >= 201103L

### C++14 新增特性

#### 核心语言特性
```cpp
// 1. 泛型 Lambda
auto add = [](auto a, auto b) { return a + b; };
add(3, 4);       // int
add(3.5, 2.1);   // double

// 2. 函数返回类型推导
auto get_value() { return 42; }

// 3. decltype(auto)
template <typename T>
auto&& forward(T&& t) {
    return static_cast<decltype(t)>(t);
}

// 4. 二进制字面量
int binary = 0b101010;  // 42

// 5. 数字分隔符
int million = 1'000'000;
double pi = 3.14159'26535;

// 6. 变量模板
template <typename T>
constexpr T pi = T(3.1415926535897932385L);

// 7. [[deprecated]] 属性
[[deprecated("Use new_func instead")]]
void old_func() {}

// 8. make_unique
auto ptr = std::make_unique<int>(42);
```

#### 标准库新增
```cpp
// <shared_mutex> - 读写锁
#include <shared_mutex>
std::shared_mutex rw_mtx;

// 标准库算法增强
// std::make_integer_sequence, std::make_index_sequence 等
```

**版本标识**：`__cplusplus` >= 201402L

### C++17 新增特性

#### 核心语言特性
```cpp
// 1. 结构化绑定
std::pair<int, int> p = {1, 2};
auto [x, y] = p;

std::map<int, std::string> m = {{1, "one"}};
for (auto& [key, value] : m) {
    std::cout << key << ": " << value << std::endl;
}

// 2. if constexpr（编译期 if）
template <typename T>
auto get_value(T t) {
    if constexpr (std::is_pointer_v<T>)
        return *t;
    else
        return t;
}

// 3. 折叠表达式
template <typename... Args>
auto sum(Args... args) {
    return (args + ... + 0);  // 右折叠

// 4. inline 变量
inline int global_counter = 0;

// 5. __has_include 预处理指令
#if __has_include(<optional>)
    #include <optional>
#endif

// 6. [[nodiscard]] 属性
[[nodiscard]] int check_status();

// 7. std::byte 类型
#include <cstddef>
std::byte b{42};
```

#### 标准库新增
```cpp
// <filesystem> - 文件系统库
#include <filesystem>
namespace fs = std::filesystem;
fs::path p = "/path/to/file";
if (fs::exists(p)) {
    std::cout << fs::file_size(p) << std::endl;
}

// <optional> - 可选值
#include <optional>
std::optional<int> result = try_parse("42");
if (result) {
    std::cout << *result << std::endl;
}

// <variant> - 类型安全的联合体
#include <variant>
std::variant<int, std::string> var;
var = 42;
var = "hello";

// <any> - 任意类型
#include <any>
std::any a = 42;
a = std::string("hello");

// <charconv> - 快速数值转换
#include <charconv>
int result;
auto [ptr, ec] = std::from_chars(str.data(), str.data() + str.size(), result);

// <memory_resource> - 多态内存资源
#include <memory_resource>
std::pmr::vector<int> vec(&std::pmr::new_delete_resource());

// <execution> - 并行算法
#include <execution>
std::sort(std::execution::par, vec.begin(), vec.end());

// <string_view> - 字符串视图（避免拷贝）
#include <string_view>
void process(std::string_view sv);

// <shared_mutex> - 读写锁（C++17）
```

**版本标识**：`__cplusplus` >= 201703L

### C++20 新增特性

#### 核心语言特性
```cpp
// 1. Concepts（概念约束）
template <typename T>
concept Integral = std::is_integral_v<T>;

template <Integral T>
T add(T a, T b) {
    return a + b;
}

// 2. Ranges（范围库）
#include <ranges>
std::vector<int> nums = {1, 2, 3, 4, 5};
auto filtered = nums | std::views::filter([](int x) { return x % 2 == 0; });
auto transformed = filtered | std::views::transform([](int x) { return x * 2; });

// 3. Modules（模块）
// export module my_module;
// export void hello();

// 4. Coroutines（协程）
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
    co_await std::suspend_always{};
    co_return;
}

// 5. 三向比较运算符
auto operator<=>(const Point&) const = default;

// 6. consteval（立即求值函数）
consteval int square(int x) { return x * x; }

// 7. constinit（常量初始化）
constinit int x = 42;

// 8. [[likely]] 和 [[unlikely]]
if (likely(condition)) {
    // 更可能执行的分支
}

// 9. Designated initializers（指定初始化器，类似 C99）
Point p {.x = 10, .y = 20};

// 10. std::span（连续序列视图）
#include <span>
void process(std::span<int> data);

// 11. std::format（类型安全的字符串格式化）
#include <format>
std::string msg = std::format("Hello, {}!", "World");

// 12. std::jthread（自动 join 的线程）
#include <thread>
std::jthread worker([] { /* work */ });
// 析构时自动 join

// 13. std::counting_semaphore（信号量）
#include <semaphore>
std::counting_semaphore<10> sem(5);
```

#### 标准库新增
```cpp
// <ranges> - 范围库
#include <ranges>

// <format> - 格式化库
#include <format>

// <concepts> - 概念定义
#include <concepts>

// <version> - 版本信息
#include <version>

// <coroutine> - 协程支持
#include <coroutine>

// <barrier> - 线程屏障
#include <barrier>

// <latch> - 线程闩
#include <latch>

// <semaphore> - 信号量
#include <semaphore>

// <compare> - 三向比较
#include <compare>

// <span> - 序列视图
#include <span>

// <numbers> - 数学常量
#include <numbers>
constexpr double pi = std::numbers::pi;
```

**版本标识**：`__cplusplus` >= 202002L

### C++23 新增特性

#### 核心语言特性
```cpp
// 1. std::expected（表示可能失败的操作）
#include <expected>
std::expected<int, std::string> divide(int a, int b) {
    if (b == 0)
        return std::unexpected("Division by zero");
    return a / b;
}

// 2. Deducing this（显式对象参数）
struct S {
    void this(this auto&& self) {
        // self 可以是引用或右值引用
    }
};

// 3. std::print（简化的输出函数）
#include <print>
std::print("Hello, {}!", "World");

// 4. auto(x) 和 auto{x}（强制拷贝）
auto copy = auto(x);

// 5. [[assume]] 属性
[[assume(x > 0)]];

// 6. if consteval（常量求值检测）
if consteval {
    // 在常量求值上下文中执行
}
```

#### 标准库新增
```cpp
// <expected> - 期望值
#include <expected>

// <print> - 输出函数
#include <print>

// <spanstream> - span 流
#include <spanstream>

// <stacktrace> - 栈追踪
#include <stacktrace>

// 标准库模块（如果编译器支持）
// import std;
```

**版本标识**：`__cplusplus` >= 202302L

## 特性查询方法

### 方法1：使用预定义宏检测

```cpp
// C++ 版本检测
#if __cplusplus >= 202302L
    // C++23 代码
#elif __cplusplus >= 202002L
    // C++20 代码
#elif __cplusplus >= 201703L
    // C++17 代码
#elif __cplusplus >= 201402L
    // C++14 代码
#elif __cplusplus >= 201103L
    // C++11 代码
#else
    // C++98/03 代码
#endif

// C 语言版本检测
#if __STDC_VERSION__ >= 202311L
    // C23 代码
#elif __STDC_VERSION__ >= 201710L
    // C17 代码
#elif __STDC_VERSION__ >= 201112L
    // C11 代码
#elif __STDC_VERSION__ >= 199901L
    // C99 代码
#else
    // C89/C90 代码
#endif
```

### 方法2：编译器特定宏

```cpp
// GCC/Clang 特性检测
#if __has_include(<filesystem>)
    #include <filesystem>
#endif

#if __has_feature(cxx_range_for)
    // 支持 range-for
#endif

// MSVC 特性检测
#ifdef _MSVC_LANG
    #if _MSVC_LANG >= 201703L
        // C++17 模式
    #endif
#endif
```

### 方法3：使用静态断言

```cpp
// 编译期检查
static_assert(__cplusplus >= 201703L, "Requires C++17 or later");

// 检查特定特性
#if __cpp_concepts >= 201907L
    // 支持 concepts
#endif
```

### 方法4：在线查询 cppreference.com

1. 访问 https://en.cppreference.com/
2. 搜索特定的函数、类或关键字
3. 查看 API 文档顶部的版本标识
   - 例如：`(since C++11)` 或 `(since C++20)`
4. 查看"Version history"部分了解详细版本信息

## 编译器支持情况

### GCC

| 编译器版本 | 默认标准 | 完整支持 |
|-----------|---------|---------|
| GCC 4.8.1 | C++98 | C++11 |
| GCC 5.0 | C++14 | C++14 |
| GCC 8.0 | C++14 | C++17 |
| GCC 11.0 | C++17 | C++20 |
| GCC 13.0 | C++17 | C++23（大部分） |

**启用特定标准**：
```bash
g++ -std=c++11
g++ -std=c++14
g++ -std=c++17
g++ -std=c++20
g++ -std=c++23  # 或 -std=gnu++23（GNU 扩展）
```

### Clang

| 编译器版本 | 默认标准 | 完整支持 |
|-----------|---------|---------|
| Clang 3.3 | C++98 | C++11 |
| Clang 3.4 | C++11 | C++14 |
| Clang 5.0 | C++14 | C++17 |
| Clang 12.0 | C++17 | C++20 |
| Clang 16.0 | C++17 | C++23（大部分） |

**启用特定标准**：
```bash
clang++ -std=c++11
clang++ -std=c++14
clang++ -std=c++17
clang++ -std=c++20
clang++ -std=c++23
```

### MSVC (Visual Studio)

| Visual Studio | 默认标准 | 完整支持 |
|----------------|---------|---------|
| VS 2010 | C++98 | C++11（部分） |
| VS 2015 | C++14 | C++14 |
| VS 2017 (15.9) | C++14 | C++17 |
| VS 2019 (16.10) | C++17 | C++20 |
| VS 2022 (17.5) | C++17 | C++23（大部分） |

**启用特定标准**：
```bash
# /std:c++17
# /std:c++20
# /std:c++23
# /std:c++latest
```

### Apple Clang (Xcode)

| Xcode 版本 | Clang 版本 | 默认标准 |
|-----------|-----------|---------|
| Xcode 10 | Clang 10.0 | C++14 |
| Xcode 12 | Clang 12.0 | C++14 |
| Xcode 13 | Clang 13.0 | C++17 |
| Xcode 14 | Clang 14.0 | C++20 |

## 版本迁移指南

### 从 C++98/03 到现代 C++

**优先替换的特性**：
1. 裸指针 → `std::unique_ptr` / `std::shared_ptr`
2. `NULL` → `nullptr`
3. `auto_ptr` → `std::unique_ptr`
4. `boost::shared_ptr` → `std::shared_ptr`
5. 旧式遍历 → 范围 for 循环
6. 函数指针 → Lambda 表达式

**示例迁移**：
```cpp
// 旧代码
void old_code() {
    int* arr = new int[100];
    for (int i = 0; i < 100; i++) {
        arr[i] = i * 2;
    }
    delete[] arr;
}

// 现代代码
void modern_code() {
    std::vector<int> arr(100);
    for (auto& item : arr) {
        item = item * 2;
    }
}
```

### 从 C++11 到 C++20

**建议使用的特性**：
1. 使用 `if constexpr` 替代 SFINAE
2. 使用结构化绑定简化代码
3. 使用 `std::filesystem` 替代文件操作
4. 使用 `std::optional` 替代指针表示可选值
5. 使用 `std::format` 替代字符串格式化
6. 使用 `std::ranges` 简化算法操作
7. 使用 concepts 约束模板
8. 使用协程简化异步代码

## 跨平台兼容性

### 平台差异

```cpp
// Windows 特定宏
#ifdef _WIN32
    // Windows 代码
#elif defined(__linux__)
    // Linux 代码
#elif defined(__APPLE__)
    // macOS 代码
#endif

// 编译器特定宏
#ifdef _MSC_VER
    // MSVC 代码
#elif defined(__GNUC__)
    // GCC 代码
#elif defined(__clang__)
    // Clang 代码
#endif
```

### 字节序问题

```cpp
// 检测字节序
#if __BYTE_ORDER__ == __ORDER_LITTLE_ENDIAN__
    // 小端序
#elif __BYTE_ORDER__ == __ORDER_BIG_ENDIAN__
    // 大端序
#endif

// 使用标准库函数
#include <cstdint>
uint32_t host_val = 0x12345678;
uint32_t net_val = htonl(host_val);  // 主机序到网络序
```

### 平台特定库

```cpp
// Windows
#ifdef _WIN32
    #include <windows.h>
#endif

// Linux/Unix
#ifdef __linux__
    #include <unistd.h>
#endif

// macOS
#ifdef __APPLE__
    #include <mach/mach.h>
#endif
```

## 最佳实践

1. **版本选择**：
   - 新项目：推荐使用最新的稳定标准（C++17/20）
   - 维护项目：考虑现有代码和编译器支持
   - 嵌入式系统：可能需要使用较老标准

2. **兼容性检查**：
   - 使用预定义宏检测编译器和版本
   - 使用静态断言确保特性可用
   - 参考编译器文档了解特性支持情况

3. **逐步迁移**：
   - 分阶段升级，每次一个标准版本
   - 充分测试确保兼容性
   - 使用 CI/CD 自动检测跨平台编译问题

4. **在线资源**：
   - cppreference.com - 权威的 C/C++ 参考文档
   - CppReference 的版本标识是查询特性支持版本的最终权威来源
