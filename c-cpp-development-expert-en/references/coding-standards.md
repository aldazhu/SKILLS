# C/C++ Coding Standards and Best Practices

## Table of Contents
- [Naming Conventions](#naming-conventions)
- [Code Style](#code-style)
- [Memory Management](#memory-management)
- [Resource Management](#resource-management)
- [Error Handling](#error-handling)
- [Concurrent Programming](#concurrent-programming)
- [Performance Optimization](#performance-optimization)
- [Security Standards](#security-standards)

## Naming Conventions

### General Rules
- **Consistency**: Maintain a consistent naming style throughout the project
- **Readability**: Names should clearly express intent; avoid abbreviations
- **Appropriate Length**: Sufficiently descriptive but not overly long

### C Language Naming Conventions
```c
// Type naming - CamelCase, starting with uppercase
typedef struct {
    int x;
    int y;
} Point;

// Function naming - underscore-separated, starting with a verb
int calculate_distance(Point a, Point b);
void process_data(const char* input);

// Variable naming - underscore-separated, lowercase
int user_count;
float total_amount;

// Constant naming - all uppercase, underscore-separated
#define MAX_SIZE 1024
const int DEFAULT_TIMEOUT = 30;

// Macro naming - all uppercase, underscore-separated
#define LOG_ERROR(msg) fprintf(stderr, "ERROR: %s\n", msg)

// Enum naming - type name starts with uppercase, values in uppercase
typedef enum {
    Color_Red,
    Color_Green,
    Color_Blue
} Color;
```

### C++ Naming Conventions
```cpp
// Class/struct naming - PascalCase
class NetworkServer { };
class UserData { };

// Member variable naming - camelCase, optional underscore prefix
class User {
public:
    std::string userName;
    int _age;  // Or use m_age
private:
    std::string _email;
};

// Public member function naming - camelCase
void sendMessage(const std::string& msg);
int getUserId() const;

// Constant naming - all uppercase
const int MAX_CONNECTIONS = 100;
constexpr double PI = 3.14159;

// Namespace naming - all lowercase
namespace database { }
namespace utils { }

// Template parameter naming - single uppercase letter or PascalCase
template <typename T>
class Container { };

template <typename Key, typename Value>
class HashMap { };
```

## Code Style

### Indentation and Formatting
```cpp
// Use 4-space indentation
if (condition) {
    statement1;
    statement2;
}

// Function definition - one parameter per line (when there are many)
void complex_function(
    int param1,
    const std::string& param2,
    double param3,
    const std::vector<int>& param4
) {
    // Implementation
}

// Function call - wrap lines when there are many arguments
result = function_with_many_params(
    arg1,
    arg2,
    arg3,
    arg4
);

// Chained calls - each operator on a new line
auto result = object
    .operation1()
    .operation2()
    .operation3();
```

### Comment Conventions
```cpp
// Single-line comment - for brief explanations
// Multi-line comment - for detailed explanations

/**
 * @brief Brief description of the function
 * 
 * @param param1 Description of parameter 1
 * @param param2 Description of parameter 2
 * @return Description of the return value
 * @throws std::runtime_error Description of the exception
 */
int calculate_value(int param1, double param2);

// TODO: Pending task
// FIXME: Issue that needs to be fixed
// HACK: Temporary workaround
// NOTE: Important note
```

### Header File Guards
```cpp
// Traditional approach
#ifndef PROJECT_FILENAME_H
#define PROJECT_FILENAME_H

// Header file contents

#endif // PROJECT_FILENAME_H

// Modern approach (C++17+)
#pragma once

// Header file contents
```

## Memory Management

### Smart Pointer Usage
```cpp
// unique_ptr - exclusive ownership
#include <memory>
std::unique_ptr<int> ptr1(new int(42));
auto ptr2 = std::make_unique<int>(42); // Recommended
ptr2 = std::move(ptr1); // Ownership transfer

// shared_ptr - shared ownership
auto shared1 = std::make_shared<int>(42);
auto shared2 = shared1; // Reference count increases
std::cout << shared1.use_count() << std::endl; // 2

// weak_ptr - prevents circular references
std::weak_ptr<int> weak = shared1;
if (auto locked = weak.lock()) {
    // Successfully locked, can access
    std::cout << *locked << std::endl;
}

// Custom deleter
auto file_ptr = std::unique_ptr<FILE, decltype(&fclose)>(
    fopen("data.txt", "r"),
    &fclose
);
```

### Avoiding Memory Leaks
```cpp
// Wrong - potential memory leak
void bad_function() {
    int* ptr = new int(42);
    if (some_condition) {
        return; // Memory leak
    }
    delete ptr;
}

// Correct - using smart pointers
void good_function() {
    auto ptr = std::make_unique<int>(42);
    if (some_condition) {
        return; // Automatically freed
    }
}

// Correct - RAII pattern
class Resource {
public:
    Resource() {
        // Acquire resource
    }
    ~Resource() {
        // Release resource
    }
};

void use_resource() {
    Resource res; // Automatically managed
}
```

## Resource Management

### RAII (Resource Acquisition Is Initialization)
```cpp
// File handling - automatic close
#include <fstream>
void process_file() {
    std::ifstream file("data.txt");
    if (!file) {
        throw std::runtime_error("Cannot open file");
    }
    // Use the file
    // Automatically closed when leaving scope
}

// Lock management - automatic release
#include <mutex>
std::mutex mtx;

void critical_section() {
    std::lock_guard<std::mutex> lock(mtx);
    // Critical section code
    // Automatically unlocked when leaving scope
}

// Use scoped_lock to manage multiple locks
std::mutex mtx1, mtx2;
void multi_lock() {
    std::scoped_lock lock(mtx1, mtx2);
    // Critical section code
}
```

### Containers and Views
```cpp
// Avoid unnecessary copies
void process_string(const std::string& str); // Pass by reference
void process_view(std::string_view sv);    // C++17 string_view

// Avoid container copies
void process_vector(const std::vector<int>& vec); // Pass by reference
void process_span(std::span<int> data);          // C++20 span

// Returning large objects
std::vector<int> generate_data() {
    return {1, 2, 3, 4, 5}; // Move semantics, no copy
}

// Accepting move semantics
void consume_data(std::vector<int>&& vec) {
    // Can modify vec
}
```

## Error Handling

### Exception Handling
```cpp
// Use exceptions to handle error conditions
#include <stdexcept>

void risky_operation() {
    if (error_condition) {
        throw std::runtime_error("Operation failed");
    }
}

// Catching exceptions
try {
    risky_operation();
} catch (const std::runtime_error& e) {
    std::cerr << "Error: " << e.what() << std::endl;
} catch (...) {
    std::cerr << "Unknown error" << std::endl;
}

// noexcept marks functions that do not throw exceptions
void safe_function() noexcept {
    // Does not throw exceptions
}
```

### Error Codes vs Exceptions
```cpp
// Error code approach - for performance-sensitive or legacy code
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

// Exception approach - recommended for modern C++
int process_with_exception(int input) {
    if (input < 0) {
        throw std::invalid_argument("Input must be positive");
    }
    return input * 2;
}

// std::optional - for operations that may fail
#include <optional>
std::optional<int> safe_divide(int a, int b) {
    if (b == 0) {
        return std::nullopt;
    }
    return a / b;
}

// Usage
auto result = safe_divide(10, 0);
if (result) {
    std::cout << "Result: " << *result << std::endl;
} else {
    std::cout << "Division by zero" << std::endl;
}
```

## Concurrent Programming

### Thread Safety
```cpp
// Mutex to protect shared data
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

// Use atomic operations to avoid locks
#include <atomic>
std::atomic<int> atomic_counter;

void safe_increment() {
    atomic_counter.fetch_add(1, std::memory_order_relaxed);
}

// Condition variable
#include <condition_variable>
std::mutex mtx;
std::condition_variable cv;
bool ready = false;

void worker() {
    std::unique_lock<std::mutex> lock(mtx);
    cv.wait(lock, [] { return ready; });
    // Perform work
}

void signal_ready() {
    {
        std::lock_guard<std::mutex> lock(mtx);
        ready = true;
    }
    cv.notify_one();
}
```

### Modern Concurrency Features
```cpp
// std::jthread (C++20) - automatic join
#include <thread>
void task() {
    // Work
}
std::jthread worker(task); // Automatically joins on destruction

// std::latch - wait for N threads to complete (C++20)
#include <latch>
std::latch completion_latch(4);
```

## Performance Optimization

### Move Semantics
```cpp
// Avoid copies, use move semantics
class Data {
public:
    Data() = default;
    Data(const Data&) = delete; // Disallow copying
    Data(Data&&) noexcept = default; // Allow moving
};

// Passing large objects
void process_big_data(const Data& data); // Read-only
void consume_big_data(Data&& data);      // Consume

// Return optimization
std::vector<int> generate_data() {
    std::vector<int> result;
    // Fill data
    return result; // Move semantics, no copy
}

// Use emplace_back to avoid temporary objects
std::vector<std::string> strings;
strings.emplace_back("Hello"); // Construct in-place
```

### Algorithm Optimization
```cpp
// Use standard algorithms
#include <algorithm>
#include <vector>

std::vector<int> data = {3, 1, 4, 1, 5, 9, 2, 6};

// Sort
std::sort(data.begin(), data.end());

// Find
auto it = std::find(data.begin(), data.end(), 5);
if (it != data.end()) {
    std::cout << "Found: " << *it << std::endl;
}

// Transform
std::transform(data.begin(), data.end(), data.begin(),
              [](int x) { return x * 2; });

// Parallel algorithms (C++17)
#include <execution>
std::sort(std::execution::par, data.begin(), data.end());
```

### Memory Pre-allocation
```cpp
std::vector<int> vec;
vec.reserve(1000); // Pre-allocate space, avoid repeated allocations

// Use reserve to avoid frequent reallocations
for (int i = 0; i < 1000; ++i) {
    vec.push_back(i); // No reallocation
}
```

## Security Standards

### Preventing Buffer Overflows
```c
// C language safe functions
#include <stdio.h>
#include <string.h>

// Use safe string functions
char buffer[100];
snprintf(buffer, sizeof(buffer), "%s", input);  // Instead of sprintf
strncpy_s(buffer, sizeof(buffer), src, count);   // Instead of strncpy

// Bounds checking
if (length < sizeof(buffer)) {
    memcpy(buffer, src, length);
}
```

### Input Validation
```cpp
// Validate input parameters
void process_input(int value) {
    if (value < 0 || value > MAX_VALUE) {
        throw std::invalid_argument("Invalid input value");
    }
    // Process valid input
}

// Validate strings
bool is_valid_string(const std::string& str) {
    return !str.empty() && 
           str.size() <= MAX_LENGTH &&
           std::all_of(str.begin(), str.end(), 
                      [](char c) { return std::isalnum(c); });
}
```

### Preventing Integer Overflow
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

### Preventing Dangling Pointers
```cpp
// Use smart pointers
#include <memory>

std::shared_ptr<Resource> create_resource() {
    return std::make_shared<Resource>();
}

// Avoid returning pointers to local objects
int* bad_function() {
    int local = 42;
    return &local; // Dangling pointer
}

// Correct approach
std::unique_ptr<int> good_function() {
    return std::make_unique<int>(42);
}
```

## Best Practices Summary

### DO - Things You Should Do
- Use smart pointers to manage dynamic memory
- Prefer `const` and `constexpr`
- Use RAII to manage resources
- Use `std::string_view` and `std::span` to avoid copies
- Use exceptions to handle error conditions
- Use standard library algorithms
- Keep functions short with a single responsibility
- Use `auto` to simplify type declarations
- Use namespaces to organize code
- Add meaningful comments

### DON'T - Things You Should Not Do
- Do not use raw pointers to manage resources
- Do not ignore exceptions
- Do not overuse global variables
- Do not use magic numbers (use constants instead)
- Do not over-optimize
- Do not use `using namespace std;` (in header files)
- Do not return references or pointers to local objects
- Do not ignore compiler warnings
- Do not repeat code (DRY principle)
- Do not optimize prematurely
