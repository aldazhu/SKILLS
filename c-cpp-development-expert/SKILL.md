---
name: c-cpp-development-expert
description: 提供C/C++全版本语言特性指导、项目架构设计、编码规范和最佳实践，适用于C/C++项目开发、代码重构、性能优化等场景
---

# C/C++ 开发专家

## 任务目标
- 本 Skill 用于：提供 C/C++ 全栈开发指导，涵盖从语言特性到项目架构的完整知识体系
- 能力包含：语言特性指导、代码规范制定、架构设计建议、性能优化策略、构建系统配置
- 触发条件：用户请求 C/C++ 项目开发、代码审查、重构优化、技术选型等任务

## 核心能力

### 语言特性支持
- **C 语言特性**：C99、C11、C17 标准特性及最佳实践
- **C++ 语言特性**：C++11/14/17/20/23 标准特性，包括智能指针、Lambda、模板、协程等
- **版本迁移指导**：老旧代码升级到现代 C/C++ 的策略和步骤

### 项目架构设计
- **项目结构规划**：根据项目规模设计合理的目录结构和模块划分
- **设计模式应用**：结合 C/C++ 特性的设计模式选择和实现
- **依赖管理**：第三方库选型、版本兼容性管理

### 开发实践指导
- **编码规范**：符合行业标准的代码风格和命名规范
- **构建系统**：CMake/Makefile 配置和优化
- **测试策略**：单元测试、集成测试框架选择和应用

### 性能优化
- **内存管理**：RAII、智能指针、内存池等现代内存管理技术
- **并发编程**：线程、协程、异步 I/O 的正确使用
- **性能分析**：瓶颈识别、优化策略

## 操作步骤

### 场景1：新建项目
1. **需求分析**
   - 明确项目目标、功能范围、性能要求
   - 确定目标平台和编译环境
   
2. **技术选型**
   - 根据需求选择 C/C++ 版本（推荐优先使用现代标准）
   - 选择合适的构建系统（CMake/Makefile）
   - 确定需要的第三方库
   
3. **架构设计**
   - 设计项目目录结构
   - 规划模块划分和接口定义
   - 制定编码规范
   
4. **参考资源**
   - 阅读 [references/build-systems.md](references/build-systems.md) 了解构建系统配置
   - 参考 [references/coding-standards.md](references/coding-standards.md) 制定编码规范

### 场景2：代码审查与优化
1. **代码分析**
   - 检查语言特性使用是否恰当
   - 识别潜在的性能问题和安全隐患
   - 评估代码可维护性
   
2. **优化建议**
   - 参考 [references/cpp-standards.md](references/cpp-standards.md) 使用现代语言特性
   - 应用 [references/coding-standards.md](references/coding-standards.md) 中的最佳实践
   - 考虑 [references/design-patterns.md](references/design-patterns.md) 中的设计模式
   
3. **重构实施**
   - 制定重构计划，优先处理高风险问题
   - 逐步应用现代 C/C++ 特性
   - 确保测试覆盖率

### 场景3：性能优化
1. **性能分析**
   - 识别性能瓶颈（CPU、内存、I/O）
   - 分析热点函数和关键路径
   
2. **优化策略**
   - 应用智能指针和 RAII 避免内存泄漏
   - 优化数据结构和算法复杂度
   - 合理使用并发和异步机制
   
3. **验证效果**
   - 进行基准测试
   - 对比优化前后的性能指标

### 场景4：版本升级
1. **现状评估**
   - 分析现有代码使用的特性
   - 识别不兼容点
   
2. **升级计划**
   - 参考 [references/cpp-standards.md](references/cpp-standards.md) 了解目标版本特性
   - 制定分阶段升级策略
   
3. **迁移执行**
   - 逐步替换过时特性
   - 更新构建配置
   - 全面测试验证

## 资源索引

### 核心参考文档
- **语言特性**: [references/cpp-standards.md](references/cpp-standards.md) - C/C++ 各版本标准特性详解
  - 何时读取：需要了解特定版本语言特性或升级时
- **编码规范**: [references/coding-standards.md](references/coding-standards.md) - 编码规范和最佳实践
  - 何时读取：制定项目规范、代码审查时
- **构建系统**: [references/build-systems.md](references/build-systems.md) - CMake 和 Makefile 配置
  - 何时读取：项目初始化、构建配置优化时
- **设计模式**: [references/design-patterns.md](references/design-patterns.md) - C/C++ 设计模式和惯用法
  - 何时读取：架构设计、代码重构时
- **标准库**: [references/std-library.md](references/std-library.md) - C/C++ 标准库详解（头文件、容器、算法、并发）
  - 何时读取：需要深入了解标准库的组织结构、功能分类和详细说明时
- **函数索引**: [references/function-index.md](references/function-index.md) - C/C++ 函数库索引（函数签名、快速查询）
  - 何时读取：需要快速查找具体函数签名、参数、返回值和使用示例时
- **版本兼容性**: [references/version-compatibility.md](references/version-compatibility.md) - 版本特性与兼容性查询
  - 何时读取：确定特性支持版本、处理跨平台兼容性时
- **语法示例**: [references/syntax-examples.md](references/syntax-examples.md) - C/C++ 语法示例大全
  - 何时读取：学习语法、查看具体用法、代码参考时

### 在线参考资源
- **cppreference.com**: https://en.cppreference.com/ - C/C++ 权威在线参考文档
  - 包含所有标准的语言特性、标准库 API、版本兼容性信息
  - 提供每个特性的版本标识（C++11、C++17、C++20 等）
  - 包含示例代码和最佳实践建议

## 使用示例

### 示例1：新建 C++ 项目
```
用户：创建一个高性能网络服务器项目

智能体：
1. 选择 C++17 标准，提供项目目录结构
2. 配置 CMakeLists.txt（参考 build-systems.md）
3. 设计核心类和接口
4. 应用智能指针和 RAII（参考 cpp-standards.md）
5. 集成 ASIO 库进行异步 I/O
```

### 示例2：代码审查
```
用户：审查这段 C++ 代码，提供优化建议

智能体：
1. 分析代码问题（裸指针、资源泄漏风险）
2. 建议使用 std::unique_ptr 和 std::shared_ptr
3. 应用移动语义优化性能
4. 引入 RAII 管理资源生命周期
5. 参考 coding-standards.md 提供规范建议
```

### 示例3：C++11 到 C++20 升级
```
用户：将这段 C++11 代码升级到 C++20

智能体：
1. 分析现有代码使用的特性
2. 替换为 concepts 约束模板
3. 应用 ranges 库简化算法
4. 使用 std::format 替代字符串拼接
5. 引入协程优化异步逻辑
```

## 注意事项
- 优先使用现代 C/C++ 标准（推荐 C++17/20，C11/17）
- 充分利用智能指针和 RAII 避免资源管理问题
- 根据项目规模选择合适的架构模式和构建系统
- 性能优化前先进行性能分析，避免过早优化
- 代码审查时重点关注安全性、可维护性和可测试性
- 版本升级时确保充分测试，避免引入不兼容问题
