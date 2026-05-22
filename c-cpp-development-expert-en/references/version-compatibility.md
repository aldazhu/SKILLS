# C/C++ Version Features and Compatibility Reference

## Table of Contents
- [Version Overview](#version-overview)
- [C Language Version Features](#c-language-version-features)
- [C++ Version Features](#c-version-features)
- [Feature Query Methods](#feature-query-methods)
- [Compiler Support](#compiler-support)
- [Version Migration Guide](#version-migration-guide)
- [Cross-Platform Compatibility](#cross-platform-compatibility)

## Version Overview

### C Language Standards Timeline

| Version | Release Year | Alias | Key Features |
|---------|-------------|-------|-------------|
| C89/C90 | 1989/1990 | ANSI C | First standard |
| C99 | 1999 | - | VLA, inline functions, complex numbers, bool |
| C11 | 2011 | - | Threads, atomic operations, anonymous structs |
| C17 | 2017 | - | C11 technical corrections |
| C23 | 2023 | (Draft) | Bit manipulation library, enhanced features |

### C++ Standards Timeline

| Version | Release Year | Alias | Key Features |
|---------|-------------|-------|-------------|
| C++98 | 1998 | - | First standard |
| C++03 | 2003 | - | Technical corrections |
| C++11 | 2011 | C++0x | Smart pointers, Lambda, move semantics, auto |
| C++14 | 2014 | C++1y | Generic Lambda, make_unique |
| C++17 | 2017 | C++1z | filesystem, optional, variant |
| C++20 | 2020 | C++2a | concepts, ranges, modules, coroutines |
| C++23 | 2023 | C++2b | std::expected, print, Deducing this |

## C Language Version Features

### C99 New Features

#### Core Language Features
```c
// 1. Variable declaration position (can be anywhere within a block)
for (int i = 0; i < 10; i++) {
    int temp = i * 2;
}

// 2. Designated initializers
struct Point { int x, y; };
struct Point p = { .y = 10, .x = 20 };

// 3. Variable-Length Arrays (VLA)
void func(int n) {
    int arr[n];  // Size determined at runtime
}

// 4. Single-line comments
// This is a single-line comment

// 5. restrict keyword
void copy(int *restrict dest, const int *restrict src, size_t n);

// 6. Inline functions
static inline int square(int x) { return x * x; }

// 7. Complex number type
#include <complex.h>
double complex z = 3.0 + 4.0 * I;

// 8. bool type
#include <stdbool.h>
bool flag = true;

// 9. __func__ predefined identifier
void my_function() {
    printf("Function: %s\n", __func__);  // Outputs "my_function"
}

// 10. Variadic macros
#define DEBUG(fmt, ...) printf(fmt, ##__VA_ARGS__)
```

#### Standard Library Additions
```c
// <stdbool.h> - bool type
// <complex.h> - Complex number operations
// <tgmath.h> - Type-generic math
// <inttypes.h> - Integer formatting
// <stdint.h> - Standard integer types (int32_t, uint64_t, etc.)
// <wchar.h> - Wide characters
```

**Version Identifier**: `__STDC_VERSION__` >= 199901L

### C11 New Features

#### Core Language Features
```c
// 1. Anonymous structs and unions
struct {
    struct { int x, y; };  // Anonymous struct
    int id;
} point;
point.x = 10;  // Direct member access

// 2. _Generic type-generic selection
#define cbrt(X) _Generic((X), \
    long double: cbrtl, \
    default: cbrt, \
    float: cbrtf \
)(X)

// 3. _Static_assert compile-time assertion
_Static_assert(sizeof(int) == 4, "int must be 4 bytes");

// 4. _Noreturn function attribute
_Noreturn void fatal_error(void) {
    abort();
}

// 5. Alignment support
_Alignas(16) int aligned_int;
size_t alignment = _Alignof(int);  // Similar to C++ alignof
```

#### Standard Library Additions
```c
// <threads.h> - Thread support
#include <threads.h>
thrd_t thread;
thrd_create(&thread, thread_func, arg);

// <stdatomic.h> - Atomic operations
#include <stdatomic.h>
atomic_int counter;
atomic_fetch_add(&counter, 1);

// <stdnoreturn.h> - noreturn macro
#include <stdnoreturn.h>
noreturn void exit_now(void);

// <uchar.h> - UTF-16/32 characters
char16_t c16 = u'中';
char32_t c32 = U'中';

// <time.h> - New time functions
timespec_get(&ts, TIME_UTC);
```

**Version Identifier**: `__STDC_VERSION__` >= 201112L

### C17 Features
- C17 is primarily a technical correction of C11
- No new language features added
- Fixed some defect reports

**Version Identifier**: `__STDC_VERSION__` >= 201710L

### C23 New Features (Draft)

#### Expected New Features
```c
// 1. Bit manipulation library
#include <stdbit.h>
unsigned int leading_zeros = stdc_leading_zeros_ui(32);

// 2. Enhanced bool type
#include <stdbool.h>
bool true_val = true;  // Stricter bool support

// 3. Enhanced attribute syntax
[[nodiscard]] int check_status(void);

// 4. Binary literals
int binary = 0b101010;  // Similar to C++14

// 5. Digit separators
int million = 1'000'000;  // Similar to C++14
```

**Version Identifier**: `__STDC_VERSION__` >= 202311L (draft value)

## C++ Version Features

### C++11 New Features

#### Core Language Features
```cpp
// 1. Type deduction
auto x = 42;  // int
auto y = 3.14;  // double
decltype(x) z = x;  // int

// 2. Range-based for loop
std::vector<int> vec = {1, 2, 3, 4, 5};
for (auto& item : vec) {
    item *= 2;
}

// 3. Lambda expressions
auto add = [](int a, int b) { return a + b; };
int result = add(3, 4);  // 7

// 4. Smart pointers
std::unique_ptr<int> ptr(new int(42));
std::shared_ptr<int> shared = std::make_shared<int>(42);

// 5. Move semantics
std::string str1 = "Hello";
std::string str2 = std::move(str1);  // str1 is left empty

// 6. nullptr
void func(int* ptr);
func(nullptr);  // Replaces NULL

// 7. Rvalue references
void process(std::string&& str);  // Accepts rvalues

// 8. noexcept
void safe_func() noexcept;  // Declares no exceptions thrown

// 9. constexpr
constexpr int square(int x) { return x * x; }
constexpr int val = square(5);  // Compile-time evaluation

// 10. Type aliases
using String = std::string;  // Replaces typedef
template <typename T>
using Vec = std::vector<T>;

// 11. Initializer lists
std::vector<int> vec = {1, 2, 3, 4, 5};

// 12. Delegating constructors
class MyClass {
public:
    MyClass(int x) : x_(x) {}
    MyClass() : MyClass(0) {}  // Delegation
};

// 13. Inheriting constructors
class Derived : public Base {
public:
    using Base::Base;  // Inherit constructors
};

// 14. override and final
class Base {
public:
    virtual void func() {}
    virtual void final_func() final {}
};

class Derived : public Base {
public:
    void func() override {}  // Explicit override
    // void final_func() {}  // Compile error: cannot override final function
};

// 15. Deleted default functions
class NoCopy {
public:
    NoCopy(const NoCopy&) = delete;  // Disable copy
    NoCopy& operator=(const NoCopy&) = delete;
};
```

#### Standard Library Additions
```cpp
// <thread> - Thread support
#include <thread>
std::thread t([] { /* work */ });
t.join();

// <mutex> - Mutual exclusion
#include <mutex>
std::mutex mtx;
std::lock_guard<std::mutex> lock(mtx);

// <condition_variable> - Condition variables
#include <condition_variable>
std::condition_variable cv;

// <atomic> - Atomic operations
#include <atomic>
std::atomic<int> counter(0);

// <future> - Future and Promise
#include <future>
std::future<int> fut = std::async([] { return 42; });
int result = fut.get();

// <chrono> - Time library
#include <chrono>
auto now = std::chrono::system_clock::now();

// <regex> - Regular expressions
#include <regex>
std::regex pattern(R"(\d+)");
std::smatch match;

// <array> - Fixed-size array
#include <array>
std::array<int, 5> arr = {1, 2, 3, 4, 5};

// <unordered_map>, <unordered_set> - Hash containers
#include <unordered_map>
std::unordered_map<int, std::string> umap;

// <tuple> - Tuples
#include <tuple>
auto t = std::make_tuple(1, "hello", 3.14);

// <memory> - Smart pointers
#include <memory>
std::unique_ptr<int> uptr = std::make_unique<int>(42);
std::shared_ptr<int> sptr = std::make_shared<int>(42);

// <functional> - Function objects
#include <functional>
std::function<int(int)> f = [](int x) { return x * 2; };

// <type_traits> - Type traits
#include <type_traits>
static_assert(std::is_integral<int>::value);

// <random> - Random number generators
#include <random>
std::mt19937 gen(std::random_device{}());
std::uniform_int_distribution<> dis(1, 100);
```

**Version Identifier**: `__cplusplus` >= 201103L

### C++14 New Features

#### Core Language Features
```cpp
// 1. Generic Lambda
auto add = [](auto a, auto b) { return a + b; };
add(3, 4);       // int
add(3.5, 2.1);   // double

// 2. Function return type deduction
auto get_value() { return 42; }

// 3. decltype(auto)
template <typename T>
auto&& forward(T&& t) {
    return static_cast<decltype(t)>(t);
}

// 4. Binary literals
int binary = 0b101010;  // 42

// 5. Digit separators
int million = 1'000'000;
double pi = 3.14159'26535;

// 6. Variable templates
template <typename T>
constexpr T pi = T(3.1415926535897932385L);

// 7. [[deprecated]] attribute
[[deprecated("Use new_func instead")]]
void old_func() {}

// 8. make_unique
auto ptr = std::make_unique<int>(42);
```

#### Standard Library Additions
```cpp
// <shared_mutex> - Read-write lock
#include <shared_mutex>
std::shared_mutex rw_mtx;

// Standard library algorithm enhancements
// std::make_integer_sequence, std::make_index_sequence, etc.
```

**Version Identifier**: `__cplusplus` >= 201402L

### C++17 New Features

#### Core Language Features
```cpp
// 1. Structured bindings
std::pair<int, int> p = {1, 2};
auto [x, y] = p;

std::map<int, std::string> m = {{1, "one"}};
for (auto& [key, value] : m) {
    std::cout << key << ": " << value << std::endl;
}

// 2. if constexpr (compile-time if)
template <typename T>
auto get_value(T t) {
    if constexpr (std::is_pointer_v<T>)
        return *t;
    else
        return t;
}

// 3. Fold expressions
template <typename... Args>
auto sum(Args... args) {
    return (args + ... + 0);  // Right fold

// 4. inline variables
inline int global_counter = 0;

// 5. __has_include preprocessor directive
#if __has_include(<optional>)
    #include <optional>
#endif

// 6. [[nodiscard]] attribute
[[nodiscard]] int check_status();

// 7. std::byte type
#include <cstddef>
std::byte b{42};
```

#### Standard Library Additions
```cpp
// <filesystem> - Filesystem library
#include <filesystem>
namespace fs = std::filesystem;
fs::path p = "/path/to/file";
if (fs::exists(p)) {
    std::cout << fs::file_size(p) << std::endl;
}

// <optional> - Optional values
#include <optional>
std::optional<int> result = try_parse("42");
if (result) {
    std::cout << *result << std::endl;
}

// <variant> - Type-safe union
#include <variant>
std::variant<int, std::string> var;
var = 42;
var = "hello";

// <any> - Any type
#include <any>
std::any a = 42;
a = std::string("hello");

// <charconv> - Fast numeric conversions
#include <charconv>
int result;
auto [ptr, ec] = std::from_chars(str.data(), str.data() + str.size(), result);

// <memory_resource> - Polymorphic memory resources
#include <memory_resource>
std::pmr::vector<int> vec(&std::pmr::new_delete_resource());

// <execution> - Parallel algorithms
#include <execution>
std::sort(std::execution::par, vec.begin(), vec.end());

// <string_view> - String view (avoids copying)
#include <string_view>
void process(std::string_view sv);

// <shared_mutex> - Read-write lock (C++17)
```

**Version Identifier**: `__cplusplus` >= 201703L

### C++20 New Features

#### Core Language Features
```cpp
// 1. Concepts (concept constraints)
template <typename T>
concept Integral = std::is_integral_v<T>;

template <Integral T>
T add(T a, T b) {
    return a + b;
}

// 2. Ranges (ranges library)
#include <ranges>
std::vector<int> nums = {1, 2, 3, 4, 5};
auto filtered = nums | std::views::filter([](int x) { return x % 2 == 0; });
auto transformed = filtered | std::views::transform([](int x) { return x * 2; });

// 3. Modules
// export module my_module;
// export void hello();

// 4. Coroutines
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

// 5. Three-way comparison operator
auto operator<=>(const Point&) const = default;

// 6. consteval (immediate evaluation function)
consteval int square(int x) { return x * x; }

// 7. constinit (constant initialization)
constinit int x = 42;

// 8. [[likely]] and [[unlikely]]
if (likely(condition)) {
    // Branch more likely to execute
}

// 9. Designated initializers (similar to C99)
Point p {.x = 10, .y = 20};

// 10. std::span (contiguous sequence view)
#include <span>
void process(std::span<int> data);

// 11. std::format (type-safe string formatting)
#include <format>
std::string msg = std::format("Hello, {}!", "World");

// 12. std::jthread (auto-joining thread)
#include <thread>
std::jthread worker([] { /* work */ });
// Automatically joins on destruction

// 13. std::counting_semaphore (semaphore)
#include <semaphore>
std::counting_semaphore<10> sem(5);
```

#### Standard Library Additions
```cpp
// <ranges> - Ranges library
#include <ranges>

// <format> - Formatting library
#include <format>

// <concepts> - Concept definitions
#include <concepts>

// <version> - Version information
#include <version>

// <coroutine> - Coroutine support
#include <coroutine>

// <barrier> - Thread barrier
#include <barrier>

// <latch> - Thread latch
#include <latch>

// <semaphore> - Semaphore
#include <semaphore>

// <compare> - Three-way comparison
#include <compare>

// <span> - Sequence view
#include <span>

// <numbers> - Mathematical constants
#include <numbers>
constexpr double pi = std::numbers::pi;
```

**Version Identifier**: `__cplusplus` >= 202002L

### C++23 New Features

#### Core Language Features
```cpp
// 1. std::expected (represents operations that may fail)
#include <expected>
std::expected<int, std::string> divide(int a, int b) {
    if (b == 0)
        return std::unexpected("Division by zero");
    return a / b;
}

// 2. Deducing this (explicit object parameter)
struct S {
    void this(this auto&& self) {
        // self can be a reference or rvalue reference
    }
};

// 3. std::print (simplified output function)
#include <print>
std::print("Hello, {}!", "World");

// 4. auto(x) and auto{x} (forced copy)
auto copy = auto(x);

// 5. [[assume]] attribute
[[assume(x > 0)]];

// 6. if consteval (constant evaluation detection)
if consteval {
    // Executed in constant evaluation context
}
```

#### Standard Library Additions
```cpp
// <expected> - Expected values
#include <expected>

// <print> - Output functions
#include <print>

// <spanstream> - Span streams
#include <spanstream>

// <stacktrace> - Stack traces
#include <stacktrace>

// Standard library modules (if supported by compiler)
// import std;
```

**Version Identifier**: `__cplusplus` >= 202302L

## Feature Query Methods

### Method 1: Using Predefined Macros

```cpp
// C++ version detection
#if __cplusplus >= 202302L
    // C++23 code
#elif __cplusplus >= 202002L
    // C++20 code
#elif __cplusplus >= 201703L
    // C++17 code
#elif __cplusplus >= 201402L
    // C++14 code
#elif __cplusplus >= 201103L
    // C++11 code
#else
    // C++98/03 code
#endif

// C language version detection
#if __STDC_VERSION__ >= 202311L
    // C23 code
#elif __STDC_VERSION__ >= 201710L
    // C17 code
#elif __STDC_VERSION__ >= 201112L
    // C11 code
#elif __STDC_VERSION__ >= 199901L
    // C99 code
#else
    // C89/C90 code
#endif
```

### Method 2: Compiler-Specific Macros

```cpp
// GCC/Clang feature detection
#if __has_include(<filesystem>)
    #include <filesystem>
#endif

#if __has_feature(cxx_range_for)
    // range-for supported
#endif

// MSVC feature detection
#ifdef _MSVC_LANG
    #if _MSVC_LANG >= 201703L
        // C++17 mode
    #endif
#endif
```

### Method 3: Using Static Assertions

```cpp
// Compile-time check
static_assert(__cplusplus >= 201703L, "Requires C++17 or later");

// Check for specific features
#if __cpp_concepts >= 201907L
    // concepts supported
#endif
```

### Method 4: Online Query via cppreference.com

1. Visit https://en.cppreference.com/
2. Search for a specific function, class, or keyword
3. Check the version identifier at the top of the API documentation
   - For example: `(since C++11)` or `(since C++20)`
4. Check the "Version history" section for detailed version information

## Compiler Support

### GCC

| Compiler Version | Default Standard | Full Support |
|-----------------|-----------------|-------------|
| GCC 4.8.1 | C++98 | C++11 |
| GCC 5.0 | C++14 | C++14 |
| GCC 8.0 | C++14 | C++17 |
| GCC 11.0 | C++17 | C++20 |
| GCC 13.0 | C++17 | C++23 (mostly) |

**Enable specific standard**:
```bash
g++ -std=c++11
g++ -std=c++14
g++ -std=c++17
g++ -std=c++20
g++ -std=c++23  # or -std=gnu++23 (GNU extensions)
```

### Clang

| Compiler Version | Default Standard | Full Support |
|-----------------|-----------------|-------------|
| Clang 3.3 | C++98 | C++11 |
| Clang 3.4 | C++11 | C++14 |
| Clang 5.0 | C++14 | C++17 |
| Clang 12.0 | C++17 | C++20 |
| Clang 16.0 | C++17 | C++23 (mostly) |

**Enable specific standard**:
```bash
clang++ -std=c++11
clang++ -std=c++14
clang++ -std=c++17
clang++ -std=c++20
clang++ -std=c++23
```

### MSVC (Visual Studio)

| Visual Studio | Default Standard | Full Support |
|--------------|-----------------|-------------|
| VS 2010 | C++98 | C++11 (partial) |
| VS 2015 | C++14 | C++14 |
| VS 2017 (15.9) | C++14 | C++17 |
| VS 2019 (16.10) | C++17 | C++20 |
| VS 2022 (17.5) | C++17 | C++23 (mostly) |

**Enable specific standard**:
```bash
# /std:c++17
# /std:c++20
# /std:c++23
# /std:c++latest
```

### Apple Clang (Xcode)

| Xcode Version | Clang Version | Default Standard |
|--------------|--------------|-----------------|
| Xcode 10 | Clang 10.0 | C++14 |
| Xcode 12 | Clang 12.0 | C++14 |
| Xcode 13 | Clang 13.0 | C++17 |
| Xcode 14 | Clang 14.0 | C++20 |

## Version Migration Guide

### From C++98/03 to Modern C++

**Priority replacements**:
1. Raw pointers → `std::unique_ptr` / `std::shared_ptr`
2. `NULL` → `nullptr`
3. `auto_ptr` → `std::unique_ptr`
4. `boost::shared_ptr` → `std::shared_ptr`
5. Old-style iteration → Range-based for loop
6. Function pointers → Lambda expressions

**Migration example**:
```cpp
// Old code
void old_code() {
    int* arr = new int[100];
    for (int i = 0; i < 100; i++) {
        arr[i] = i * 2;
    }
    delete[] arr;
}

// Modern code
void modern_code() {
    std::vector<int> arr(100);
    for (auto& item : arr) {
        item = item * 2;
    }
}
```

### From C++11 to C++20

**Recommended features to adopt**:
1. Use `if constexpr` to replace SFINAE
2. Use structured bindings to simplify code
3. Use `std::filesystem` to replace file operations
4. Use `std::optional` to replace pointers for optional values
5. Use `std::format` to replace string formatting
6. Use `std::ranges` to simplify algorithm operations
7. Use concepts to constrain templates
8. Use coroutines to simplify asynchronous code

## Cross-Platform Compatibility

### Platform Differences

```cpp
// Windows-specific macros
#ifdef _WIN32
    // Windows code
#elif defined(__linux__)
    // Linux code
#elif defined(__APPLE__)
    // macOS code
#endif

// Compiler-specific macros
#ifdef _MSC_VER
    // MSVC code
#elif defined(__GNUC__)
    // GCC code
#elif defined(__clang__)
    // Clang code
#endif
```

### Endianness Issues

```cpp
// Detect endianness
#if __BYTE_ORDER__ == __ORDER_LITTLE_ENDIAN__
    // Little-endian
#elif __BYTE_ORDER__ == __ORDER_BIG_ENDIAN__
    // Big-endian
#endif

// Using standard library functions
#include <cstdint>
uint32_t host_val = 0x12345678;
uint32_t net_val = htonl(host_val);  // Host byte order to network byte order
```

### Platform-Specific Libraries

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

## Best Practices

1. **Version Selection**:
   - New projects: Recommend using the latest stable standard (C++17/20)
   - Maintenance projects: Consider existing code and compiler support
   - Embedded systems: May need to use older standards

2. **Compatibility Checks**:
   - Use predefined macros to detect compiler and version
   - Use static assertions to ensure feature availability
   - Refer to compiler documentation for feature support details

3. **Gradual Migration**:
   - Upgrade in stages, one standard version at a time
   - Thoroughly test to ensure compatibility
   - Use CI/CD to automatically detect cross-platform compilation issues

4. **Online Resources**:
   - cppreference.com - Authoritative C/C++ reference documentation
   - CppReference's version identifiers are the definitive authoritative source for querying feature support versions
