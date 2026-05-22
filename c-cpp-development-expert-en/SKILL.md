---
name: c-cpp-development-expert
description: Provides guidance on C/C++ language features across all versions, project architecture design, coding standards and best practices, applicable to C/C++ project development, code refactoring, performance optimization and more
---

# C/C++ Development Expert

## Objectives
- This Skill is for: providing full-stack C/C++ development guidance, covering a complete knowledge system from language features to project architecture
- Capabilities include: language feature guidance, coding standard definition, architecture design advice, performance optimization strategies, build system configuration
- Trigger conditions: when the user requests C/C++ project development, code review, refactoring and optimization, technology selection, etc.

## Core Capabilities

### Language Feature Support
- **C Language Features**: C99, C11, C17 standard features and best practices
- **C++ Language Features**: C++11/14/17/20/23 standard features, including smart pointers, Lambda, templates, coroutines, etc.
- **Version Migration Guidance**: strategies and steps for upgrading legacy code to modern C/C++

### Project Architecture Design
- **Project Structure Planning**: designing reasonable directory structures and module divisions based on project scale
- **Design Pattern Application**: selecting and implementing design patterns suited to C/C++ features
- **Dependency Management**: third-party library selection, version compatibility management

### Development Practice Guidance
- **Coding Standards**: code style and naming conventions conforming to industry standards
- **Build Systems**: CMake/Makefile configuration and optimization
- **Testing Strategy**: unit testing, integration testing framework selection and application

### Performance Optimization
- **Memory Management**: modern memory management techniques including RAII, smart pointers, memory pools
- **Concurrent Programming**: correct usage of threads, coroutines, and async I/O
- **Performance Analysis**: bottleneck identification, optimization strategies

## Operational Steps

### Scenario 1: New Project
1. **Requirements Analysis**
   - Clarify project goals, functional scope, performance requirements
   - Determine target platform and compilation environment
   
2. **Technology Selection**
   - Choose C/C++ version based on requirements (prefer modern standards)
   - Select appropriate build system (CMake/Makefile)
   - Determine required third-party libraries
   
3. **Architecture Design**
   - Design project directory structure
   - Plan module divisions and interface definitions
   - Establish coding standards
   
4. **Reference Resources**
   - Read [references/build-systems.md](references/build-systems.md) for build system configuration
   - Refer to [references/coding-standards.md](references/coding-standards.md) for coding standards

### Scenario 2: Code Review and Optimization
1. **Code Analysis**
   - Check whether language features are used appropriately
   - Identify potential performance issues and security vulnerabilities
   - Evaluate code maintainability
   
2. **Optimization Suggestions**
   - Refer to [references/cpp-standards.md](references/cpp-standards.md) for modern language features
   - Apply best practices from [references/coding-standards.md](references/coding-standards.md)
   - Consider design patterns from [references/design-patterns.md](references/design-patterns.md)
   
3. **Refactoring Implementation**
   - Create a refactoring plan, prioritizing high-risk issues
   - Gradually apply modern C/C++ features
   - Ensure test coverage

### Scenario 3: Performance Optimization
1. **Performance Analysis**
   - Identify performance bottlenecks (CPU, memory, I/O)
   - Analyze hotspot functions and critical paths
   
2. **Optimization Strategy**
   - Apply smart pointers and RAII to prevent memory leaks
   - Optimize data structures and algorithm complexity
   - Use concurrency and async mechanisms appropriately
   
3. **Verify Results**
   - Run benchmarks
   - Compare pre- and post-optimization performance metrics

### Scenario 4: Version Upgrade
1. **Current State Assessment**
   - Analyze features used in existing code
   - Identify incompatibilities
   
2. **Upgrade Plan**
   - Refer to [references/cpp-standards.md](references/cpp-standards.md) for target version features
   - Create a phased upgrade strategy
   
3. **Migration Execution**
   - Gradually replace deprecated features
   - Update build configurations
   - Run comprehensive tests

## Resource Index

### Core Reference Documents
- **Language Features**: [references/cpp-standards.md](references/cpp-standards.md) - Detailed C/C++ standard features by version
  - When to read: when you need to understand specific version features or during upgrades
- **Coding Standards**: [references/coding-standards.md](references/coding-standards.md) - Coding standards and best practices
  - When to read: when establishing project standards or during code reviews
- **Build Systems**: [references/build-systems.md](references/build-systems.md) - CMake and Makefile configuration
  - When to read: during project initialization or build configuration optimization
- **Design Patterns**: [references/design-patterns.md](references/design-patterns.md) - C/C++ design patterns and idioms
  - When to read: during architecture design or code refactoring
- **Standard Library**: [references/std-library.md](references/std-library.md) - C/C++ standard library reference (headers, containers, algorithms, concurrency)
  - When to read: when you need in-depth understanding of standard library organization, feature categories, and detailed descriptions
- **Function Index**: [references/function-index.md](references/function-index.md) - C/C++ function library index (function signatures, quick reference)
  - When to read: when you need to quickly look up specific function signatures, parameters, return values, and usage examples
- **Version Compatibility**: [references/version-compatibility.md](references/version-compatibility.md) - Version features and compatibility reference
  - When to read: when determining feature support versions or handling cross-platform compatibility
- **Syntax Examples**: [references/syntax-examples.md](references/syntax-examples.md) - Comprehensive C/C++ syntax examples
  - When to read: when learning syntax, viewing specific usage, or for code reference

### Online Reference Resources
- **cppreference.com**: https://en.cppreference.com/ - Authoritative C/C++ online reference documentation
  - Contains language features, standard library APIs, and version compatibility information for all standards
  - Provides version identifiers for each feature (C++11, C++17, C++20, etc.)
  - Includes example code and best practice recommendations

## Usage Examples

### Example 1: New C++ Project
```
User: Create a high-performance network server project

Agent:
1. Select C++17 standard, provide project directory structure
2. Configure CMakeLists.txt (refer to build-systems.md)
3. Design core classes and interfaces
4. Apply smart pointers and RAII (refer to cpp-standards.md)
5. Integrate ASIO library for async I/O
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

### Example 3: C++11 to C++20 Upgrade
```
User: Upgrade this C++11 code to C++20

Agent:
1. Analyze features used in existing code
2. Replace with concepts-constrained templates
3. Apply ranges library to simplify algorithms
4. Use std::format to replace string concatenation
5. Introduce coroutines to optimize async logic
```

## Notes
- Prefer modern C/C++ standards (recommend C++17/20, C11/17)
- Make full use of smart pointers and RAII to avoid resource management issues
- Choose appropriate architecture patterns and build systems based on project scale
- Perform profiling before performance optimization to avoid premature optimization
- Focus on security, maintainability, and testability during code reviews
- Ensure thorough testing during version upgrades to avoid introducing incompatibilities
