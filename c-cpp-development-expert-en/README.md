# C/C++ Development Expert

A comprehensive C/C++ development assistant Skill that provides a complete knowledge system from language features to project architecture, helping developers efficiently complete C/C++ project development, code refactoring, performance optimization, and more.

## Core Skills

### 1. Language Feature Support
- **Full C Version Support**: C89/C90, C99, C11, C17, C23 standard feature details
- **Full C++ Version Support**: C++98, C++11, C++14, C++17, C++20, C++23 standard features
- **Version Migration Guidance**: strategies and steps for upgrading legacy code to modern C/C++
- **Latest Feature Application**: smart pointers, Lambda expressions, coroutines, Concepts, Ranges, etc.

### 2. Project Architecture Design
- **Project Structure Planning**: designing reasonable directory structures and module divisions based on project scale
- **Build System Configuration**: CMake and Makefile configuration and optimization
- **Dependency Management**: third-party library selection, version compatibility management
- **Design Pattern Application**: selecting and implementing design patterns suited to C/C++ features
- **Modular Design**: code organization and interface design best practices

### 3. Coding Standards and Best Practices
- **Naming Conventions**: C/C++ naming conventions and style guides
- **Code Style**: indentation, comments, formatting standards
- **Memory Management**: RAII, smart pointers, resource management best practices
- **Error Handling**: exception handling, error codes, std::optional usage
- **Security Standards**: preventing buffer overflows, integer overflows, dangling pointers
- **Performance Optimization**: move semantics, avoiding unnecessary copies, algorithm optimization

### 4. Standard Library Mastery
- **C Standard Library**: complete header file classification and function reference
- **C++ Container Library**: vector, map, unordered_map, array, etc.
- **Algorithm Library**: sorting, searching, transformation, parallel algorithms
- **Smart Pointers and Memory**: unique_ptr, shared_ptr, weak_ptr
- **Concurrency and Synchronization**: thread, mutex, atomic, future
- **Input/Output**: iostream, filesystem, stringstream
- **String Handling**: string, string_view, regex
- **Modern C++ Features**: optional, variant, any, tuple
- **C++20 Additions**: ranges, format, coroutines

### 5. Version Compatibility Management
- **Version Feature Queries**: language features and added library comparison tables for each standard
- **Compiler Support Status**: GCC, Clang, MSVC version comparison
- **Feature Detection Methods**: predefined macros, static assertions, online queries
- **Version Migration Guide**: steps for upgrading from old standards to modern standards
- **Cross-Platform Compatibility**: platform difference handling, endianness issues

### 6. Design Patterns and Idioms
- **Creational Patterns**: Singleton, Factory, Builder
- **Structural Patterns**: Adapter, Decorator, Proxy
- **Behavioral Patterns**: Observer, Strategy, Command
- **C++ Idioms**: RAII, Pimpl, CRTP, Type Erasure
- **Modern C++ Patterns**: type-safe unions, expected value pattern, coroutine patterns

## Trigger Methods

### Direct Trigger Scenarios

Users can directly trigger the Skill usage through the following methods:

#### 1. New Project
```
User: Create a high-performance network server project
User: Help me design a C++ project structure
User: Create a new cross-platform C++ application
```

#### 2. Code Review and Optimization
```
User: Review this C++ code and provide optimization suggestions
User: Help me check what issues this code has
User: How to optimize the performance of this C++ function
```

#### 3. Language Feature Queries
```
User: Explain C++11 smart pointers
User: What new features does C++17 have
User: How to use C++20 concepts
User: What are the differences between C99 and C11
```

#### 4. Standard Library Usage
```
User: How to use std::filesystem
User: How to use std::optional
User: Recommend a container suitable for this scenario
User: What sorting algorithms does the C++ standard library have
```

#### 5. Version Migration
```
User: Upgrade this C++11 code to C++20
User: How to migrate from C++98 to modern C++
User: What should I be aware of when upgrading C code to C17
```

#### 6. Performance Optimization
```
User: How to optimize this program's performance
User: C++ memory management best practices
User: How to avoid unnecessary copies
User: What should I be aware of in concurrent programming
```

#### 7. Design Patterns
```
User: Implement a singleton pattern
User: What design pattern should I use here
User: What is CRTP and how to use it
```

#### 8. Build System Configuration
```
User: Help me write a CMakeLists.txt
User: Configure a project supporting C++17
User: How to manage third-party library dependencies
```

### Implicit Trigger Scenarios

The agent should automatically invoke this Skill when the user's request involves the following:

1. **C/C++ Code Generation**: generating C/C++ code, algorithm implementations, data structures
2. **Technology Selection**: choosing appropriate C/C++ features, standard libraries, third-party libraries
3. **Architecture Design**: designing modules, interfaces, project structures
4. **Troubleshooting**: debugging C/C++ code, locating compilation errors, runtime errors
5. **Best Practice Consultation**: C/C++ coding standards, performance optimization advice, security practices
6. **Cross-Platform Development**: handling platform differences, compiler compatibility issues

## Skill Characteristics

### 1. Comprehensiveness
- Covers all major standard versions of C and C++
- Encompasses language features, standard library, design patterns, build systems
- Provides a complete knowledge system from beginner to advanced

### 2. Authoritativeness
- References authoritative documentation such as cppreference.com
- Provides accurate standard version information
- Includes compiler support comparison

### 3. Practicality
- All examples are ready-to-use code
- Provides best practices and pitfall avoidance guides
- Includes version migration and cross-platform compatibility guidance

### 4. Modernity
- Emphasizes modern C/C++ standards (C++17/20, C17)
- Highlights smart pointers, RAII, move semantics and other modern features
- Includes the latest C++20/23 feature applications

### 5. Intelligence
- Provides guidance through the agent's natural language understanding capabilities
- No manual configuration needed, works out of the box
- Automatically selects the most appropriate solution based on context

## Target Audience

- **C/C++ Beginners**: learning language features and best practices
- **Intermediate Developers**: improving code quality and architecture design skills
- **Advanced Developers**: obtaining in-depth technical guidance and optimization advice
- **Project Leaders**: conducting technology selection and architecture design
- **Code Reviewers**: identifying issues and providing improvement suggestions

## Online References

- **cppreference.com**: https://en.cppreference.com/ - Authoritative C/C++ online reference documentation
  - Contains language features, standard library APIs, and version compatibility information for all standards
  - Provides version identifiers and example code for each feature

## Usage Examples

### Example 1: New Project
```
User: Create a high-performance network server project

Agent:
1. Select C++17 standard
2. Design project directory structure
3. Configure CMakeLists.txt (refer to build-systems.md)
4. Design core classes and interfaces
5. Apply smart pointers and RAII (refer to cpp-standards.md)
6. Integrate ASIO library for async I/O
```

### Example 2: Code Review
```
User: Review this C++ code and provide optimization suggestions

Agent:
1. Analyze code issues (raw pointers, resource leak risks)
2. Suggest using std::unique_ptr and std::shared_ptr
3. Apply move semantics for performance optimization
4. Introduce RAII for resource lifecycle management
5. Refer to coding-standards.md for convention suggestions
```

### Example 3: Version Upgrade
```
User: Upgrade this C++11 code to C++20

Agent:
1. Analyze features used in existing code
2. Replace with concepts-constrained templates
3. Apply ranges library to simplify algorithms
4. Use std::format to replace string concatenation
5. Introduce coroutines to optimize async logic
```

## Skill Structure

```
cpp-development-agent/
├── SKILL.md                          # Main entry document
├── README.md                         # Skill description document (this file)
└── references/
    ├── cpp-standards.md             # C/C++ language standard feature details
    ├── coding-standards.md          # Coding standards and best practices
    ├── build-systems.md             # Build system reference
    ├── design-patterns.md           # Design patterns and idioms
    ├── std-library.md               # Standard library reference
    └── version-compatibility.md     # Version compatibility guide
```

## Version History

- **v1.0** - Initial version, including basic language features, coding standards, build systems, and design patterns
- **v1.1** - Integrated cppreference.com, added standard library reference and version compatibility documents

## License

This Skill is designed specifically for C/C++ development assistance, providing complete technical guidance and best practices.
