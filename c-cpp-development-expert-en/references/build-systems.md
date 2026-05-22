# C/C++ Build Systems Reference

## Table of Contents
- [CMake](#cmake)
- [Makefile](#makefile)
- [Project Structure](#project-structure)
- [Dependency Management](#dependency-management)
- [Best Practices](#best-practices)

## CMake

### Basic Configuration
```cmake
# Minimum version requirement
cmake_minimum_required(VERSION 3.15)

# Project information
project(MyProject
    VERSION 1.0.0
    LANGUAGES CXX
)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Output directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
```

### Executable Files
```cmake
# Simple executable
add_executable(my_app main.cpp)

# Executable with source file list
set(SOURCES
    src/main.cpp
    src/util.cpp
    src/network.cpp
)
add_executable(my_app ${SOURCES})

# Include header directories
target_include_directories(my_app PRIVATE
    ${CMAKE_SOURCE_DIR}/include
    ${CMAKE_SOURCE_DIR}/third_party/include
)

# Link libraries
target_link_libraries(my_app PRIVATE
    pthread
    dl
    OpenSSL::SSL
    OpenSSL::Crypto
)

# Compile options
target_compile_options(my_app PRIVATE
    -Wall -Wextra -Wpedantic
    -O3
)

# Define macros
target_compile_definitions(my_app PRIVATE
    VERSION_MAJOR=${PROJECT_VERSION_MAJOR}
    VERSION_MINOR=${PROJECT_VERSION_MINOR}
)
```

### Library Files
```cmake
# Static library
add_library(my_static_lib STATIC
    src/lib.cpp
    src/helper.cpp
)
target_include_directories(my_static_lib PUBLIC
    ${CMAKE_SOURCE_DIR}/include
)

# Shared library
add_library(my_shared_lib SHARED
    src/lib.cpp
    src/helper.cpp
)
target_include_directories(my_shared_lib PUBLIC
    ${CMAKE_SOURCE_DIR}/include
)

# Set library version
set_target_properties(my_shared_lib PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
)
```

### Finding Packages
```cmake
# Find standard libraries
find_package(Threads REQUIRED)
find_package(OpenSSL REQUIRED)

# Use packages
target_link_libraries(my_app PRIVATE
    Threads::Threads
    OpenSSL::SSL
    OpenSSL::Crypto
)

# Find third-party libraries
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

### Installation Rules
```cmake
# Install executable
install(TARGETS my_app
    RUNTIME DESTINATION bin
)

# Install library files
install(TARGETS my_shared_lib
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)

# Install header files
install(DIRECTORY include/
    DESTINATION include
    FILES_MATCHING PATTERN "*.h"
)

# Install configuration files
install(FILES config/app.conf
    DESTINATION etc/myapp
)
```

### Complete Example Project
```cmake
cmake_minimum_required(VERSION 3.15)
project(MyProject VERSION 1.0.0 LANGUAGES CXX)

# C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Compile options
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    add_compile_options(-g -O0 -DDEBUG)
else()
    add_compile_options(-O3 -DNDEBUG)
endif()

# Find dependencies
find_package(Threads REQUIRED)
find_package(Boost REQUIRED COMPONENTS filesystem system)

# Main program
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

# Test program
enable_testing()
add_subdirectory(tests)
```

## Makefile

### Basic Makefile
```makefile
# Variable definitions
CC = gcc
CXX = g++
CFLAGS = -Wall -Wextra -std=c11
CXXFLAGS = -Wall -Wextra -std=c++17
LDFLAGS = -lpthread -ldl

# Target files
TARGET = my_app
SOURCES = $(wildcard src/*.cpp)
OBJECTS = $(SOURCES:.cpp=.o)

# Default target
all: $(TARGET)

# Linking
$(TARGET): $(OBJECTS)
	$(CXX) $(CXXFLAGS) -o $@ $^ $(LDFLAGS)

# Compilation
%.o: %.cpp
	$(CXX) $(CXXFLAGS) -c $< -o $@

# Clean
clean:
	rm -f $(OBJECTS) $(TARGET)

# Install
install: $(TARGET)
	install -m 755 $(TARGET) /usr/local/bin/

# Phony targets
.PHONY: all clean install
```

### Advanced Makefile
```makefile
# Directory structure
SRC_DIR = src
BUILD_DIR = build
BIN_DIR = bin
INCLUDE_DIR = include

# Compiler
CC = gcc
CXX = g++
AR = ar

# Compile options
CFLAGS = -Wall -Wextra -std=c11 -I$(INCLUDE_DIR)
CXXFLAGS = -Wall -Wextra -std=c++17 -I$(INCLUDE_DIR)
LDFLAGS = -lpthread -ldl

# Debug/Release mode
DEBUG ?= 0
ifeq ($(DEBUG), 1)
    CFLAGS += -g -O0 -DDEBUG
    CXXFLAGS += -g -O0 -DDEBUG
else
    CFLAGS += -O3 -DNDEBUG
    CXXFLAGS += -O3 -DNDEBUG
endif

# Source files
C_SOURCES = $(wildcard $(SRC_DIR)/*.c)
CXX_SOURCES = $(wildcard $(SRC_DIR)/*.cpp)

# Object files
C_OBJECTS = $(C_SOURCES:$(SRC_DIR)/%.c=$(BUILD_DIR)/%.o)
CXX_OBJECTS = $(CXX_SOURCES:$(SRC_DIR)/%.cpp=$(BUILD_DIR)/%.o)
OBJECTS = $(C_OBJECTS) $(CXX_OBJECTS)

# Target program
TARGET = $(BIN_DIR)/my_app

# Library files
LIB_STATIC = $(BIN_DIR)/libmyapp.a
LIB_SHARED = $(BIN_DIR)/libmyapp.so

# Default target
all: $(TARGET)

# Create directories
$(BUILD_DIR) $(BIN_DIR):
	mkdir -p $@

# Compile C files
$(BUILD_DIR)/%.o: $(SRC_DIR)/%.c | $(BUILD_DIR)
	$(CC) $(CFLAGS) -c $< -o $@

# Compile C++ files
$(BUILD_DIR)/%.o: $(SRC_DIR)/%.cpp | $(BUILD_DIR)
	$(CXX) $(CXXFLAGS) -c $< -o $@

# Link executable
$(TARGET): $(OBJECTS) | $(BIN_DIR)
	$(CXX) $(CXXFLAGS) -o $@ $^ $(LDFLAGS)

# Create static library
$(LIB_STATIC): $(OBJECTS) | $(BIN_DIR)
	$(AR) rcs $@ $^

# Create shared library
$(LIB_SHARED): $(OBJECTS) | $(BIN_DIR)
	$(CXX) -shared -o $@ $^ $(LDFLAGS)

# Clean
clean:
	rm -rf $(BUILD_DIR) $(BIN_DIR)

# Dependencies
-include $(OBJECTS:.o=.d)

# Generate dependencies
$(BUILD_DIR)/%.d: $(SRC_DIR)/%.c | $(BUILD_DIR)
	$(CC) $(CFLAGS) -MM -MT $(BUILD_DIR)/$*.o $< -MF $@

$(BUILD_DIR)/%.d: $(SRC_DIR)/%.cpp | $(BUILD_DIR)
	$(CXX) $(CXXFLAGS) -MM -MT $(BUILD_DIR)/$*.o $< -MF $@

.PHONY: all clean
```

## Project Structure

### Standard Project Structure
```
project/
├── CMakeLists.txt          # CMake main configuration file
├── README.md               # Project description
├── .gitignore              # Git ignore file
├── build/                  # Build directory (not committed)
├── include/                # Public header files
│   ├── app.h
│   └── utils.h
├── src/                    # Source files
│   ├── main.cpp
│   ├── app.cpp
│   └── utils.cpp
├── tests/                  # Test files
│   ├── test_app.cpp
│   └── CMakeLists.txt
├── docs/                   # Documentation
│   └── api.md
├── scripts/                # Build scripts
│   └── build.sh
├── config/                 # Configuration files
│   └── app.conf
└── third_party/            # Third-party libraries
    └── lib/
```

### Modular Project Structure
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

## Dependency Management

### FetchContent (CMake 3.11+)
```cmake
include(FetchContent)

# Download and configure third-party library
FetchContent_Declare(
    spdlog
    GIT_REPOSITORY https://github.com/gabime/spdlog.git
    GIT_TAG v1.10.0
)

FetchContent_MakeAvailable(spdlog)

# Use library
target_link_libraries(my_app PRIVATE spdlog::spdlog)
```

### ExternalProject
```cmake
include(ExternalProject)

# Download and build external project
ExternalProject_Add(
    external_lib
    GIT_REPOSITORY https://github.com/example/lib.git
    GIT_TAG master
    PREFIX ${CMAKE_BINARY_DIR}/external
    CMAKE_ARGS -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}
)

# Depend on external project
add_dependencies(my_app external_lib)
```

### Package Manager Integration
```cmake
# Conan integration
find_package(conan REQUIRED)
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

# Vcpkg integration
find_package(PkgConfig REQUIRED)
pkg_check_modules(LIB REQUIRED libname)

target_include_directories(my_app PRIVATE ${LIB_INCLUDE_DIRS})
target_link_libraries(my_app PRIVATE ${LIB_LIBRARIES})
```

## Best Practices

### 1. Out-of-source Build
```bash
# Recommended approach
mkdir build
cd build
cmake ..
make

# Not recommended (pollutes source directory)
cmake .
make
```

### 2. Build Types
```bash
# Debug mode
cmake -DCMAKE_BUILD_TYPE=Debug ..

# Release mode
cmake -DCMAKE_BUILD_TYPE=Release ..

# RelWithDebInfo mode
cmake -DCMAKE_BUILD_TYPE=RelWithDebInfo ..
```

### 3. Cross-Platform Support
```cmake
# Platform detection
if(WIN32)
    add_compile_options(/W4)
elseif(UNIX)
    add_compile_options(-Wall -Wextra)
endif()

# Architecture detection
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    message(STATUS "64-bit build")
else()
    message(STATUS "32-bit build")
endif()
```

### 4. Compiler Feature Detection
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

### 5. Target Properties
```cmake
# Set output name
set_target_properties(my_app PROPERTIES
    OUTPUT_NAME "myapp"
    VERSION ${PROJECT_VERSION}
)

# Set compile definitions
target_compile_definitions(my_app PRIVATE
    MYAPP_VERSION="${PROJECT_VERSION}"
)

# Set include directories
target_include_directories(my_app PRIVATE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)
```

### 6. Installation and Packaging
```cmake
# Installation rules
install(TARGETS my_app
    RUNTIME DESTINATION bin
)

install(DIRECTORY include/
    DESTINATION include
    FILES_MATCHING PATTERN "*.h"
)

# Packaging configuration
set(CPACK_GENERATOR "DEB;RPM;TGZ")
set(CPACK_PACKAGE_NAME "myapp")
set(CPACK_PACKAGE_VERSION ${PROJECT_VERSION})
include(CPack)
```

### 7. Test Integration
```cmake
# Enable testing
enable_testing()

# Add tests
add_executable(test_main tests/test_main.cpp)
target_link_libraries(test_main PRIVATE my_app GTest::gtest)

# Register tests
add_test(NAME UnitTests COMMAND test_main)

# Test timeout
set_tests_properties(UnitTests PROPERTIES TIMEOUT 30)
```

### 8. Static Analysis
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

### 9. Documentation Generation
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

### 10. Profiling
```cmake
# Add profiling option
option(ENABLE_PROFILING "Enable profiling support" OFF)

if(ENABLE_PROFILING)
    target_compile_options(my_app PRIVATE -pg)
    target_link_options(my_app PRIVATE -pg)
endif()
```

## Common Issues

### 1. Dependency Not Found
```cmake
# Specify search path
set(CMAKE_PREFIX_PATH "/path/to/lib")
find_package(MyLib REQUIRED)
```
