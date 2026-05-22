# C/C++ Language Standards Detailed Guide

## Table of Contents
- [C Language Standards](#c-language-standards)
- [C++ Language Standards](#c-language-standards-1)
- [Version Migration Guide](#version-migration-guide)
- [Common Feature Comparisons](#common-feature-comparisons)

## C Language Standards

### C99 Features
- **Variable Declarations**: Support for declaring variables anywhere within a code block
- **Inline Functions**: `inline` keyword for function inlining
- **Variable-Length Arrays**: Arrays with runtime-determined size `int n = 10; int arr[n];`
- **Complex Types**: `complex.h` provides complex number support
- **Boolean Type**: `stdbool.h` provides `bool`, `true`, `false`
- **Designated Initializers**: Initialize struct members by name
- **Predefined Macros**: `__func__` to get the current function name

**Examples**:
```c
// Variable declaration
for (int i = 0; i < 10; i++) { /* ... */ }

// Designated initializer
struct Point { int x, y; };
struct Point p = { .x = 10, .y = 20 };

// Variable-length array
void process(int n) {
    int data[n]; // Size determined at runtime
}
```

### C11 Features
- **Thread Support**: `threads.h` provides thread creation and management
- **Atomic Operations**: `stdatomic.h` provides atomic types
- **Anonymous Structs**: Support for anonymous structs and unions
- **Generic Selection**: `_Generic` macro for type-based selection
- **Static Assertions**: `_Static_assert` for compile-time assertions
- **Alignment Support**: `alignof`, `_Alignas`

**Examples**:
```c
// Generic selection
#define cbrt(X) _Generic((X), \
    long double: cbrtl, \
    default: cbrt, \
    float: cbrtf \
)(X)

// Static assertion
_Static_assert(sizeof(int) == 4, "int must be 4 bytes");

// Anonymous struct
struct { int x, y; } point;
point.x = 10; // Direct member access

// Atomic operation
#include <stdatomic.h>
atomic_int counter;
atomic_fetch_add(&counter, 1);
```

### C17 Features
- **C11 Corrections**: Fixes issues in C11, no major new features
- **Removal of Deprecated Features**: Such as the `gets()` function

## C++ Language Standards

### C++11 Core Features
- **Smart Pointers**: `std::unique_ptr`, `std::shared_ptr`, `std::weak_ptr`
- **Move Semantics**: Rvalue references `T&&` and `std::move`
- **Lambda Expressions**: Anonymous function support `[captures](params) -> return_type { body }`
- **Automatic Type Deduction**: `auto` keyword
- **Range-based for Loop**: `for (auto& item : container)`
- **nullptr**: Null pointer constant, replacing NULL
- **constexpr**: Compile-time constant expressions
- **Type Aliases**: `using` alias syntax
- **tuple**: `std::tuple` generic tuple
- **Regular Expressions**: `<regex>` library

**Examples**:
```cpp
// Smart pointers
#include <memory>
std::unique_ptr<int> ptr(new int(42));
std::shared_ptr<int> shared = std::make_shared<int>(42);

// Move semantics
std::string str1 = "Hello";
std::string str2 = std::move(str1); // str1 is left empty

// Lambda
auto add = [](int a, int b) { return a + b; };
int result = add(3, 4); // 7

// auto type deduction
auto x = 42; // int
auto y = 3.14; // double

// Range-based for
std::vector<int> nums = {1, 2, 3, 4, 5};
for (auto& num : nums) {
    num *= 2;
}

// nullptr
void func(int* ptr);
func(nullptr); // Replaces NULL

// constexpr
constexpr int square(int x) { return x * x; }
constexpr int value = square(5); // Computed at compile time
```

### C++14 Features
- **Generic Lambdas**: `auto` parameter support
- **Function Return Type Deduction**: `auto` as return type
- **Variable Templates**: Template variable support
- **Binary Literals**: `0b101010`
- **Digit Separators**: `1'000'000`
- **make_unique**: `std::make_unique` factory function
- **shared_timed_mutex**: Shared timed mutex

**Examples**:
```cpp
// Generic lambda
auto add = [](auto a, auto b) { return a + b; };
add(3, 4); // int
add(3.5, 2.1); // double

// Function return type deduction
auto get_value() { return 42; }

// Binary literal
int binary = 0b101010; // 42

// Digit separator
int million = 1'000'000;
double pi = 3.14159'26535;

// make_unique
#include <memory>
auto ptr = std::make_unique<int>(42);
```

### C++17 Features
- **Structured Bindings**: `auto [x, y] = pair;`
- **if constexpr**: Compile-time conditionals
- **Fold Expressions**: Parameter pack expansion
- **inline Variables**: Inline variable definitions
- **std::filesystem**: Filesystem library
- **std::optional**: Potentially empty values
- **std::variant**: Type-safe union
- **std::any**: Type-erased value container
- **Parallel Algorithms**: Execution policy parameters

**Examples**:
```cpp
// Structured bindings
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

// Fold expression
template <typename... Args>
auto sum(Args... args) {
    return (args + ... + 0);
}

// inline variable
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

### C++20 Features
- **Concepts**: Concept constraints for template parameters
- **Ranges**: Ranges library and pipe operations
- **Coroutines**: Coroutine support with `co_await`, `co_yield`, `co_return`
- **Modules**: Module system replacing header files
- **Three-way Comparison**: `<=>` operator
- **std::format**: Type-safe string formatting
- **std::span**: Contiguous sequence view
- **std::jthread**: Auto-joining thread
- **consteval**: Immediate evaluation functions
- **contains**: Container membership check

**Examples**:
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
    co_await std::suspend_always{}; // Suspend
    co_return; // Return
}

// Three-way comparison
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
        // Work loop
    }
}); // Auto join

// consteval
consteval int square(int x) { return x * x; }
constexpr int result = square(5); // Must be evaluated at compile time

// contains
std::vector<int> v = {1, 2, 3};
if (v.contains(2)) {
    std::cout << "Found 2" << std::endl;
}
```

### C++23 Features
- **std::expected**: Represents operations that may fail
- **Deducing this**: Explicit object parameter
- **Standard Library Modules**: Standard library modularization support
- **View Extensions**: More ranges views
- **print**: Simplified output function

**Examples**:
```cpp
// std::expected (C++23)
#include <expected>
std::expected<int, std::string> divide(int a, int b) {
    if (b == 0)
        return std::unexpected("Division by zero");
    return a / b;
}

// Usage
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

## Version Migration Guide

### From C++98/03 to Modern C++
**Priority Replacements**:
1. Raw pointers → `std::unique_ptr` / `std::shared_ptr`
2. `NULL` → `nullptr`
3. Manual memory management (`new`/`delete`) → Smart pointers + RAII
4. `std::auto_ptr` → `std::unique_ptr`
5. `boost::shared_ptr` → `std::shared_ptr`
6. Old-style iteration → Range-based for loops
7. Function pointers → Lambda expressions

**Incremental Migration Strategy**:
```cpp
// Old code
void old_style() {
    int* arr = new int[100];
    for (int i = 0; i < 100; i++) {
        arr[i] = i * 2;
    }
    delete[] arr;
}

// Modern code
void modern_style() {
    std::vector<int> arr(100);
    for (auto& item : arr) {
        item = item * 2;
    }
}

// Old code - Resource management
void process_file() {
    FILE* f = fopen("data.txt", "r");
    if (f) {
        // Process file
        fclose(f);
    }
}

// Modern code - RAII
#include <fstream>
void process_file() {
    std::ifstream f("data.txt");
    // Process file, automatically closed
}
```

## Common Feature Comparisons

### Smart Pointer Use Cases
| Use Case | Recommended Type | Description |
|----------|-----------------|-------------|
| Exclusive ownership | `std::unique_ptr` | Non-copyable, move-only |
| Shared ownership | `std::shared_ptr` | Reference-counted, copyable |
| Circular references | `std::weak_ptr` | Observer, does not increase reference count |

### Container Selection Guide
| Requirement | Recommended Container | Time Complexity |
|-------------|----------------------|-----------------|
| Fast lookup | `std::unordered_map` | O(1) average |
| Ordered lookup | `std::map` | O(log n) |
| Random access | `std::vector` | O(1) |
| Front insertion | `std::deque` | O(1) |
| Middle insertion | `std::list` | O(1) |

### Best Practices
- Prefer `auto` to simplify type declarations, but maintain readability
- Use `std::make_unique` and `std::make_shared` to create smart pointers
- Prefer range-based for loops over traditional index-based iteration
- Prefer lambdas over function objects and function pointers
- Use `std::optional` instead of pointers to represent potentially empty values
- Use `std::string_view` to avoid string copies
- Use `std::span` to avoid container copies
- Use `[[nodiscard]]` to mark functions whose return values must be checked
