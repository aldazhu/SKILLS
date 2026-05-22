# C/C++ 构建系统详解

## 目录
- [CMake](#cmake)
- [Makefile](#makefile)
- [项目结构](#项目结构)
- [依赖管理](#依赖管理)
- [最佳实践](#最佳实践)

## CMake

### 基础配置
```cmake
# 最低版本要求
cmake_minimum_required(VERSION 3.15)

# 项目信息
project(MyProject
    VERSION 1.0.0
    LANGUAGES CXX
)

# 设置 C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 输出目录
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
```

### 可执行文件
```cmake
# 简单可执行文件
add_executable(my_app main.cpp)

# 带源文件列表的可执行文件
set(SOURCES
    src/main.cpp
    src/util.cpp
    src/network.cpp
)
add_executable(my_app ${SOURCES})

# 包含头文件目录
target_include_directories(my_app PRIVATE
    ${CMAKE_SOURCE_DIR}/include
    ${CMAKE_SOURCE_DIR}/third_party/include
)

# 链接库
target_link_libraries(my_app PRIVATE
    pthread
    dl
    OpenSSL::SSL
    OpenSSL::Crypto
)

# 编译选项
target_compile_options(my_app PRIVATE
    -Wall -Wextra -Wpedantic
    -O3
)

# 定义宏
target_compile_definitions(my_app PRIVATE
    VERSION_MAJOR=${PROJECT_VERSION_MAJOR}
    VERSION_MINOR=${PROJECT_VERSION_MINOR}
)
```

### 库文件
```cmake
# 静态库
add_library(my_static_lib STATIC
    src/lib.cpp
    src/helper.cpp
)
target_include_directories(my_static_lib PUBLIC
    ${CMAKE_SOURCE_DIR}/include
)

# 共享库
add_library(my_shared_lib SHARED
    src/lib.cpp
    src/helper.cpp
)
target_include_directories(my_shared_lib PUBLIC
    ${CMAKE_SOURCE_DIR}/include
)

# 设置库版本
set_target_properties(my_shared_lib PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
)
```

### 查找包
```cmake
# 查找标准库
find_package(Threads REQUIRED)
find_package(OpenSSL REQUIRED)

# 使用包
target_link_libraries(my_app PRIVATE
    Threads::Threads
    OpenSSL::SSL
    OpenSSL::Crypto
)

# 查找第三方库
find_package(Boost 1.71 REQUIRED COMPONENTS
    filesystem
    system
    thread
)

target_link_libraries(my_app PRIVATE
    Boost::filesystem
    Boost::system
    Boost::thread
)
```

### 安装规则
```cmake
# 安装可执行文件
install(TARGETS my_app
    RUNTIME DESTINATION bin
)

# 安装库文件
install(TARGETS my_shared_lib
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)

# 安装头文件
install(DIRECTORY include/
    DESTINATION include
    FILES_MATCHING PATTERN "*.h"
)

# 安装配置文件
install(FILES config/app.conf
    DESTINATION etc/myapp
)
```

### 完整示例项目
```cmake
cmake_minimum_required(VERSION 3.15)
project(MyProject VERSION 1.0.0 LANGUAGES CXX)

# C++ 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 编译选项
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_compile_options(-g -O0 -DDEBUG)
else()
    add_compile_options(-O3 -DNDEBUG)
endif()

# 查找依赖
find_package(Threads REQUIRED)
find_package(Boost REQUIRED COMPONENTS filesystem system)

# 主程序
add_executable(my_app
    src/main.cpp
    src/app.cpp
    src/utils.cpp
)
target_include_directories(my_app PRIVATE
    ${CMAKE_SOURCE_DIR}/include
)
target_link_libraries(my_app PRIVATE
    Threads::Threads
    Boost::filesystem
    Boost::system
)

# 测试程序
enable_testing()
add_subdirectory(tests)
```

## Makefile

### 基础 Makefile
```makefile
# 变量定义
CC = gcc
CXX = g++
CFLAGS = -Wall -Wextra -std=c11
CXXFLAGS = -Wall -Wextra -std=c++17
LDFLAGS = -lpthread -ldl

# 目标文件
TARGET = my_app
SOURCES = $(wildcard src/*.cpp)
OBJECTS = $(SOURCES:.cpp=.o)

# 默认目标
all: $(TARGET)

# 链接
$(TARGET): $(OBJECTS)
	$(CXX) $(CXXFLAGS) -o $@ $^ $(LDFLAGS)

# 编译
%.o: %.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

# 清理
clean:
	rm -f $(OBJECTS) $(TARGET)

# 安装
install: $(TARGET)
	install -m 755 $(TARGET) /usr/local/bin/

# Phony 目标
.PHONY: all clean install
```

### 高级 Makefile
```makefile
# 目录结构
SRC_DIR = src
BUILD_DIR = build
BIN_DIR = bin
INCLUDE_DIR = include

# 编译器
CC = gcc
CXX = g++
AR = ar

# 编译选项
CFLAGS = -Wall -Wextra -std=c11 -I$(INCLUDE_DIR)
CXXFLAGS = -Wall -Wextra -std=c++17 -I$(INCLUDE_DIR)
LDFLAGS = -lpthread -ldl

# 调试/发布模式
DEBUG ?= 0
ifeq ($(DEBUG), 1)
    CFLAGS += -g -O0 -DDEBUG
    CXXFLAGS += -g -O0 -DDEBUG
else
    CFLAGS += -O3 -DNDEBUG
    CXXFLAGS += -O3 -DNDEBUG
endif

# 源文件
C_SOURCES = $(wildcard $(SRC_DIR)/*.c)
CXX_SOURCES = $(wildcard $(SRC_DIR)/*.cpp)

# 目标文件
C_OBJECTS = $(C_SOURCES:$(SRC_DIR)/%.c=$(BUILD_DIR)/%.o)
CXX_OBJECTS = $(CXX_SOURCES:$(SRC_DIR)/%.cpp=$(BUILD_DIR)/%.o)
OBJECTS = $(C_OBJECTS) $(CXX_OBJECTS)

# 目标程序
TARGET = $(BIN_DIR)/my_app

# 库文件
LIB_STATIC = $(BIN_DIR)/libmyapp.a
LIB_SHARED = $(BIN_DIR)/libmyapp.so

# 默认目标
all: $(TARGET)

# 创建目录
$(BUILD_DIR) $(BIN_DIR):
	mkdir -p $@

# 编译 C 文件
$(BUILD_DIR)/%.o: $(SRC_DIR)/%.c | $(BUILD_DIR)
	$(CC) $(CFLAGS) -c $< -o $@

# 编译 C++ 文件
$(BUILD_DIR)/%.o: $(SRC_DIR)/%.cpp | $(BUILD_DIR)
	$(CXX) $(CXXFLAGS) -c $< -o $@

# 链接可执行文件
$(TARGET): $(OBJECTS) | $(BIN_DIR)
	$(CXX) $(CXXFLAGS) -o $@ $^ $(LDFLAGS)

# 创建静态库
$(LIB_STATIC): $(OBJECTS) | $(BIN_DIR)
	$(AR) rcs $@ $^

# 创建共享库
$(LIB_SHARED): $(OBJECTS) | $(BIN_DIR)
	$(CXX) -shared -o $@ $^ $(LDFLAGS)

# 清理
clean:
	rm -rf $(BUILD_DIR) $(BIN_DIR)

# 依赖关系
-include $(OBJECTS:.o=.d)

# 生成依赖
$(BUILD_DIR)/%.d: $(SRC_DIR)/%.c | $(BUILD_DIR)
	$(CC) $(CFLAGS) -MM -MT $(BUILD_DIR)/$*.o $< -MF $@

$(BUILD_DIR)/%.d: $(SRC_DIR)/%.cpp | $(BUILD_DIR)
	$(CXX) $(CXXFLAGS) -MM -MT $(BUILD_DIR)/$*.o $< -MF $@

.PHONY: all clean
```

## 项目结构

### 标准项目结构
```
project/
├── CMakeLists.txt          # CMake 主配置文件
├── README.md               # 项目说明
├── .gitignore              # Git 忽略文件
├── build/                  # 构建目录（不提交）
├── include/                # 公共头文件
│   ├── app.h
│   └── utils.h
├── src/                    # 源文件
│   ├── main.cpp
│   ├── app.cpp
│   └── utils.cpp
├── tests/                  # 测试文件
│   ├── test_app.cpp
│   └── CMakeLists.txt
├── docs/                   # 文档
│   └── api.md
├── scripts/                # 构建脚本
│   └── build.sh
├── config/                 # 配置文件
│   └── app.conf
└── third_party/            # 第三方库
    └── lib/
```

### 模块化项目结构
```
myapp/
├── CMakeLists.txt
├── src/
│   ├── CMakeLists.txt
│   ├── core/
│   │   ├── CMakeLists.txt
│   │   ├── utils.cpp
│   │   └── utils.h
│   ├── network/
│   │   ├── CMakeLists.txt
│   │   ├── client.cpp
│   │   └── server.cpp
│   └── app/
│       ├── CMakeLists.txt
│       └── main.cpp
├── include/
│   └── myapp/
│       ├── config.h
│       └── version.h
└── tests/
    └── CMakeLists.txt
```

## 依赖管理

### FetchContent (CMake 3.11+)
```cmake
include(FetchContent)

# 下载并配置第三方库
FetchContent_Declare(
    spdlog
    GIT_REPOSITORY https://github.com/gabime/spdlog.git
    GIT_TAG v1.10.0
)

FetchContent_MakeAvailable(spdlog)

# 使用库
target_link_libraries(my_app PRIVATE spdlog::spdlog)
```

### ExternalProject
```cmake
include(ExternalProject)

# 下载并构建外部项目
ExternalProject_Add(
    external_lib
    GIT_REPOSITORY https://github.com/example/lib.git
    GIT_TAG master
    PREFIX ${CMAKE_BINARY_DIR}/external
    CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}
)

# 依赖外部项目
add_dependencies(my_app external_lib)
```

### 包管理器集成
```cmake
# Conan 集成
find_package(conan REQUIRED)
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

# Vcpkg 集成
find_package(PkgConfig REQUIRED)
pkg_check_modules(LIB REQUIRED libname)

target_include_directories(my_app PRIVATE ${LIB_INCLUDE_DIRS})
target_link_libraries(my_app PRIVATE ${LIB_LIBRARIES})
```

## 最佳实践

### 1. Out-of-source 构建
```bash
# 推荐方式
mkdir build
cd build
cmake ..
make

# 不推荐方式（污染源代码目录）
cmake .
make
```

### 2. 构建类型
```bash
# Debug 模式
cmake -DCMAKE_BUILD_TYPE=Debug ..

# Release 模式
cmake -DCMAKE_BUILD_TYPE=Release ..

# RelWithDebInfo 模式
cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo ..
```

### 3. 跨平台支持
```cmake
# 平台检测
if(WIN32)
    add_compile_options(/W4)
elseif(UNIX)
    add_compile_options(-Wall -Wextra)
endif()

# 架构检测
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    message(STATUS "64-bit build")
else()
    message(STATUS "32-bit build")
endif()
```

### 4. 编译器特性检测
```cmake
include(CheckCXXSourceCompiles)

check_cxx_source_compiles("
    #include <memory>
    int main() {
        auto ptr = std::make_unique<int>(42);
        return 0;
    }
" HAS_MAKE_UNIQUE)

if(HAS_MAKE_UNIQUE)
    message(STATUS "Compiler supports std::make_unique")
endif()
```

### 5. 目标属性
```cmake
# 设置输出名称
set_target_properties(my_app PROPERTIES
    OUTPUT_NAME "myapp"
    VERSION ${PROJECT_VERSION}
)

# 设置编译定义
target_compile_definitions(my_app PRIVATE
    MYAPP_VERSION="${PROJECT_VERSION}"
)

# 设置包含目录
target_include_directories(my_app PRIVATE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
```

### 6. 安装和打包
```cmake
# 安装规则
install(TARGETS my_app
    RUNTIME DESTINATION bin
)

install(DIRECTORY include/
    DESTINATION include
    FILES_MATCHING PATTERN "*.h"
)

# 打包配置
set(CPACK_GENERATOR "DEB;RPM;TGZ")
set(CPACK_PACKAGE_NAME "myapp")
set(CPACK_PACKAGE_VERSION ${PROJECT_VERSION})
include(CPack)
```

### 7. 测试集成
```cmake
# 启用测试
enable_testing()

# 添加测试
add_executable(test_main tests/test_main.cpp)
target_link_libraries(test_main PRIVATE my_app GTest::gtest)

# 注册测试
add_test(NAME UnitTests COMMAND test_main)

# 测试超时
set_tests_properties(UnitTests PROPERTIES TIMEOUT 30)
```

### 8. 静态分析
```cmake
# Clang-Tidy
find_program(CLANG_TIDY clang-tidy)
if(CLANG_TIDY)
    set_target_properties(my_app PROPERTIES
        CXX_CLANG_TIDY "${CLANG_TIDY}")
endif()

# Cppcheck
find_program(CPPCHECK cppcheck)
if(CPPCHECK)
    add_custom_target(cppcheck
        COMMAND ${CPPCHECK}
            --enable=all
            --inconclusive
            --suppress=missingIncludeSystem
            ${CMAKE_SOURCE_DIR}/src
    )
endif()
```

### 9. 文档生成
```cmake
# Doxygen
find_package(Doxygen)
if(DOXYGEN_FOUND)
    set(DOXYGEN_IN ${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in)
    set(DOXYGEN_OUT ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile)
    
    configure_file(${DOXYGEN_IN} ${DOXYGEN_OUT})
    
    add_custom_target(doc
        COMMAND ${DOXYGEN_EXECUTABLE} ${DOXYGEN_OUT}
        WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
        COMMENT "Generating API documentation with Doxygen"
        VERBATIM
    )
endif()
```

### 10. 性能分析
```cmake
# 添加性能分析选项
option(ENABLE_PROFILING "Enable profiling support" OFF)

if(ENABLE_PROFILING)
    target_compile_options(my_app PRIVATE -pg)
    target_link_options(my_app PRIVATE -pg)
endif()
```

## 常见问题

### 1. 依赖找不到
```cmake
# 指定查找路径
set(CMAKE_PREFIX_PATH "/path/to/lib")
find_package(MyLib REQUIRED)
```

### 2. 头文件找不到
```cmake
# 添加包含目录
target_include_directories(my_app PRIVATE
    ${CMAKE_SOURCE_DIR}/include
    ${EXTERNAL_LIB_INCLUDE_DIRS}
)
```

### 3. 链接错误
```cmake
# 确保库链接顺序
target_link_libraries(my_app PRIVATE
    library_a
    library_b
    library_c
)
```

### 4. 多线程支持
```cmake
find_package(Threads REQUIRED)
target_link_libraries(my_app PRIVATE Threads::Threads)
```

### 5. 静态链接
```cmake
set(CMAKE_FIND_LIBRARY_SUFFIXES ".a")
set(BUILD_SHARED_LIBS OFF)
```
