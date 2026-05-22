# C/C++ 标准库详解

## 目录
- [C 标准库](#c-标准库)
- [C++ 标准库](#c-标准库-1)
- [版本差异](#版本差异)
- [最佳实践](#最佳实践)

## C 标准库

### 标准头文件

#### 输入/输出
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<stdio.h>` | 标准输入输出 | printf, scanf, fopen, fread, fwrite |
| `<wchar.h>` | 宽字符输入输出 | wprintf, wscanf, fwprintf |

#### 字符处理
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<ctype.h>` | 字符分类与转换 | isalpha, isdigit, toupper, tolower |
| `<wctype.h>` | 宽字符分类 | iswalpha, iswdigit, towupper, towlower |

#### 字符串处理
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<string.h>` | 字符串操作 | strcpy, strcmp, strlen, strstr |
| `<wchar.h>` | 宽字符串操作 | wcscpy, wcscmp, wcslen |

#### 数学函数
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<math.h>` | 数学运算 | sin, cos, sqrt, pow, exp |
| `<complex.h>` | 复数运算 | creal, cimag, cexp, csqrt |
| `<fenv.h>` | 浮点环境 | fegetround, fesetround |

#### 工具函数
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<stdlib.h>` | 通用工具 | malloc, free, atoi, rand, qsort |
| `<time.h>` | 时间处理 | time, localtime, strftime |

#### 诊断
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<assert.h>` | 断言 | assert |
| `<errno.h>` | 错误码 | errno |

#### 变量参数
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<stdarg.h>` | 可变参数 | va_start, va_arg, va_end |
| `<stdargs.h>` | 同上（旧名） | 同上 |

#### 内存管理
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<stdlib.h>` | 动态内存 | malloc, calloc, realloc, free |
| `<string.h>` | 内存操作 | memcpy, memset, memcmp |

#### 信号处理
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<signal.h>` | 信号处理 | signal, raise |
| `<setjmp.h>` | 非本地跳转 | setjmp, longjmp |

#### 局部化
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<locale.h>` | 本地化 | setlocale, localeconv |

#### 数据类型
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<stddef.h>` | 标准定义 | size_t, ptrdiff_t, NULL, offsetof |
| `<stdint.h>` | 标准整数 | int32_t, uint64_t, INT32_MAX |
| `<stdbool.h>` | 布尔类型 | bool, true, false (C99) |
| `<inttypes.h>` | 整数格式化 | PRId32, SCNu64 |

#### 日期与时间
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<time.h>` | 时间处理 | time, clock, difftime |

#### 原子操作 (C11)
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<stdatomic.h>` | 原子操作 | atomic_load, atomic_store, atomic_fetch_add |

#### 线程支持 (C11)
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<threads.h>` | 线程管理 | thrd_create, thrd_join, mtx_lock, cnd_wait |
| `<time.h>` | 线程时间 | thrd_sleep |

#### 通用类型 (C11)
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<stdalign.h>` | 对齐 | alignas, alignof |
| `<stdnoreturn.h>` | 不返回函数 | noreturn |

#### 边界检查函数 (C11)
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<stdckdint.h>` | 溢出检查 | ckd_add, ckd_mul, ckd_sub |

### C 标准库详细说明

#### 1. 输入输出库 (`<stdio.h>`)

**文件操作函数**

| 函数 | 功能 | 返回值 |
|------|------|--------|
| `fopen()` | 打开文件 | FILE* 或 NULL |
| `fclose()` | 关闭文件 | 0 (成功) 或 EOF |
| `fflush()` | 刷新缓冲区 | 0 (成功) 或 EOF |
| `fseek()` | 设置文件位置 | 0 (成功) 或非零 |
| `ftell()` | 获取当前位置 | 当前位置 或 -1L |
| `rewind()` | 重置文件位置到开头 | 无 |
| `fgetpos()` | 获取文件位置 | 0 (成功) 或非零 |
| `fsetpos()` | 设置文件位置 | 0 (成功) 或非零 |
| `remove()` | 删除文件 | 0 (成功) 或非零 |
| `rename()` | 重命名文件 | 0 (成功) 或非零 |
| `tmpfile()` | 创建临时文件 | FILE* 或 NULL |
| `tmpnam()` | 生成临时文件名 | 文件名 或 NULL |

**格式化输出**

| 函数 | 功能 | 示例 |
|------|------|------|
| `printf()` | 格式化输出到 stdout | `printf("Value: %d\n", x)` |
| `fprintf()` | 格式化输出到流 | `fprintf(fp, "Data: %f\n", y)` |
| `sprintf()` | 格式化输出到字符串 | `sprintf(buf, "Result: %s", str)` |
| `snprintf()` | 格式化输出到字符串(限制长度) | `snprintf(buf, 10, "Val: %d", x)` |

**格式化输入**

| 函数 | 功能 | 示例 |
|------|------|------|
| `scanf()` | 从 stdin 读取 | `scanf("%d", &value)` |
| `fscanf()` | 从流读取 | `fscanf(fp, "%f", &num)` |
| `sscanf()` | 从字符串读取 | `sscanf(str, "%d-%d", &a, &b)` |

**字符输入输出**

| 函数 | 功能 |
|------|------|
| `fputc()` | 输出字符到流 |
| `putc()` | fputc 的宏版本 |
| `putchar()` | 输出字符到 stdout |
| `fputs()` | 输出字符串到流 |
| `puts()` | 输出字符串到 stdout |
| `fgetc()` | 从流读取字符 |
| `getc()` | fgetc 的宏版本 |
| `getchar()` | 从 stdin 读取字符 |
| `fgets()` | 从流读取字符串 |
| `gets()` | 从 stdin 读取字符串（已弃用） |

**块输入输出**

| 函数 | 功能 |
|------|------|
| `fread()` | 从流读取数据块 |
| `fwrite()` | 向流写入数据块 |

**文件状态**

| 函数 | 功能 |
|------|------|
| `feof()` | 检查文件结束标志 |
| `ferror()` | 检查错误标志 |
| `clearerr()` | 清除错误和结束标志 |

#### 2. 字符串库 (`<string.h>`)

**字符串操作**

| 函数 | 功能 | 注意事项 |
|------|------|---------|
| `strcpy()` | 复制字符串 | 可能缓冲区溢出 |
| `strncpy()` | 复制指定长度 | 不保证空终止 |
| `strcat()` | 连接字符串 | 可能缓冲区溢出 |
| `strncat()` | 连接指定长度 | 保证空终止 |
| `strcmp()` | 比较字符串 | 返回负数/0/正数 |
| `strncmp()` | 比较指定长度 | 同上 |
| `strlen()` | 计算长度 | 不包含空字符 |

**字符串搜索**

| 函数 | 功能 | 返回值 |
|------|------|--------|
| `strchr()` | 查找字符 | 找到返回指针，未找到返回 NULL |
| `strrchr()` | 从后向前查找字符 | 同上 |
| `strstr()` | 查找子字符串 | 同上 |
| `strpbrk()` | 查找任意匹配字符 | 同上 |
| `strspn()` | 计算前导匹配字符数 | 返回长度 |
| `strcspn()` | 计算前导不匹配字符数 | 返回长度 |
| `strtok()` | 分割字符串 | 需要多次调用 |

**内存操作**

| 函数 | 功能 | 用途 |
|------|------|------|
| `memcpy()` | 复制内存块 | 复制不重叠区域 |
| `memmove()` | 复制内存块 | 可处理重叠区域 |
| `memset()` | 设置内存值 | 初始化内存 |
| `memcmp()` | 比较内存块 | 比较二进制数据 |
| `memchr()` | 在内存中查找字符 | 查找字节 |

#### 3. 数学库 (`<math.h>`)

**三角函数**

| 函数 | 功能 | 示例 |
|------|------|------|
| `sin()` | 正弦 | `sin(PI/2) = 1` |
| `cos()` | 余弦 | `cos(0) = 1` |
| `tan()` | 正切 | `tan(PI/4) = 1` |
| `asin()` | 反正弦 | `asin(0.5) = PI/6` |
| `acos()` | 反余弦 | `acos(0) = PI/2` |
| `atan()` | 反正切 | `atan(1) = PI/4` |
| `atan2()` | 反正切(y, x) | `atan2(1, 1) = PI/4` |

**双曲函数**

| 函数 | 功能 |
|------|------|
| `sinh()` | 双曲正弦 |
| `cosh()` | 双曲余弦 |
| `tanh()` | 双曲正切 |

**指数与对数**

| 函数 | 功能 | 示例 |
|------|------|------|
| `exp()` | e 的幂 | `exp(1) = e` |
| `log()` | 自然对数 | `log(e) = 1` |
| `log10()` | 以 10 为底的对数 | `log10(100) = 2` |
| `log2()` | 以 2 为底的对数 | `log2(8) = 3` |

**幂函数**

| 函数 | 功能 |
|------|------|
| `pow()` | 幂运算 |
| `sqrt()` | 平方根 |
| `cbrt()` | 立方根 |

**取整函数**

| 函数 | 功能 | 示例 |
|------|------|------|
| `ceil()` | 向上取整 | `ceil(2.3) = 3` |
| `floor()` | 向下取整 | `floor(2.7) = 2` |
| `round()` | 四舍五入 | `round(2.5) = 3` |
| `trunc()` | 截断 | `trunc(2.7) = 2` |

#### 4. 工具库 (`<stdlib.h>`)

**内存管理**

| 函数 | 功能 | 注意事项 |
|------|------|---------|
| `malloc()` | 分配内存 | 需要检查返回值 |
| `calloc()` | 分配并清零内存 | 初始化为 0 |
| `realloc()` | 重新分配内存 | 可能移动数据 |
| `free()` | 释放内存 | 只能释放一次 |

**程序控制**

| 函数 | 功能 |
|------|------|
| `exit()` | 正常退出程序 |
| `abort()` | 异常终止程序 |
| `atexit()` | 注册退出函数 |

**类型转换**

| 函数 | 功能 | 示例 |
|------|------|------|
| `atoi()` | 字符串转整数 | `atoi("123") = 123` |
| `atol()` | 字符串转长整数 | 同上 |
| `atof()` | 字符串转浮点数 | `atof("3.14") = 3.14` |
| `strtol()` | 字符串转长整数 | 支持错误检测 |
| `strtod()` | 字符串转双精度 | 支持错误检测 |

**随机数**

| 函数 | 功能 | 用法 |
|------|------|------|
| `rand()` | 生成随机数 | `rand() % 100` |
| `srand()` | 设置随机种子 | `srand(time(NULL))` |

**排序与搜索**

| 函数 | 功能 |
|------|------|
| `qsort()` | 快速排序 |
| `bsearch()` | 二分搜索 |

#### 5. 字符分类库 (`<ctype.h>`)

**字符分类函数**

| 函数 | 功能 | 示例 |
|------|------|------|
| `isalnum()` | 字母或数字 | `isalnum('A') = 真` |
| `isalpha()` | 字母 | `isalpha('a') = 真` |
| `isdigit()` | 数字 | `isdigit('5') = 真` |
| `islower()` | 小写字母 | `islower('z') = 真` |
| `isupper()` | 大写字母 | `isupper('Z') = 真` |
| `isspace()` | 空白字符 | `isspace(' ') = 真` |
| `isxdigit()` | 十六进制数字 | `isxdigit('A') = 真` |

**字符转换函数**

| 函数 | 功能 | 示例 |
|------|------|------|
| `tolower()` | 转为小写 | `tolower('A') = 'a'` |
| `toupper()` | 转为大写 | `toupper('a') = 'A'` |

#### 6. 时间库 (`<time.h>`)

**时间函数**

| 函数 | 功能 | 返回值 |
|------|------|--------|
| `time()` | 获取当前时间 | 时间戳 |
| `clock()` | 获取 CPU 时间 | 时钟周期数 |
| `difftime()` | 计算时间差 | 秒数 |
| `mktime()` | 转换为时间戳 | 时间戳 |

**时间转换**

| 函数 | 功能 | 用途 |
|------|------|------|
| `asctime()` | 时间结构转字符串 | 简单格式 |
| `ctime()` | 时间戳转字符串 | 简单格式 |
| `gmtime()` | 转换为 UTC 时间 | 获取 UTC |
| `localtime()` | 转换为本地时间 | 获取本地时间 |
| `strftime()` | 格式化时间字符串 | 自定义格式 |

## C++ 标准库

### 标准头文件

#### 输入输出
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<iostream>` | 标准流 | cin, cout, cerr, clog |
| `<fstream>` | 文件流 | ifstream, ofstream, fstream |
| `<sstream>` | 字符串流 | istringstream, ostringstream |
| `<iomanip>` | 格式化 | setw, setprecision |

#### 容器
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<vector>` | 动态数组 | vector |
| `<deque>` | 双端队列 | deque |
| `<array>` | 固定数组 | array (C++11) |
| `<list>` | 双向链表 | list |
| `<forward_list>` | 单向链表 | forward_list (C++11) |
| `<stack>` | 栈 | stack |
| `<queue>` | 队列 | queue, priority_queue |
| `<set>` | 有序集合 | set, multiset |
| `<map>` | 有序映射 | map, multimap |
| `<unordered_set>` | 无序集合 | unordered_set, unordered_multiset (C++11) |
| `<unordered_map>` | 无序映射 | unordered_map, unordered_multimap (C++11) |
| `<bitset>` | 位集合 | bitset |
| `<valarray>` | 值数组 | valarray |

#### 算法
| 头文件 | 用途 | 关键算法 |
|--------|------|---------|
| `<algorithm>` | 通用算法 | sort, find, transform |
| `<numeric>` | 数值算法 | accumulate, inner_product |

#### 迭代器
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<iterator>` | 迭代器 | iterator, const_iterator |
| `<memory>` | 内存管理 | shared_ptr, unique_ptr, allocator |

#### 字符串
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<string>` | 字符串类 | string, wstring |
| `<string_view>` | 字符串视图 | string_view (C++17) |
| `<regex>` | 正则表达式 | regex, smatch (C++11) |
| `<charconv>` | 字符转换 | to_chars, from_chars (C++17) |

#### 函数对象
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<functional>` | 函数对象 | function, bind, mem_fn |

#### 数值
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<complex>` | 复数 | complex |
| `<valarray>` | 值数组 | valarray |
| `<numeric>` | 数值算法 | accumulate |
| `<ratio>` | 编译期分数 | ratio (C++11) |
| `<limits>` | 数值限制 | numeric_limits |

#### 日期时间
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<chrono>` | 时间库 | duration, time_point (C++11) |
| `<ctime>` | C 时间 | time, tm |

#### 本地化
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<locale>` | 本地化 | locale |
| `<codecvt>` | 字符编码转换 | codecvt (C++11, C++17 弃用) |

#### 异常
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<exception>` | 异常类 | exception |
| `<stdexcept>` | 标准异常 | runtime_error, logic_error |
| `<system_error>` | 系统错误 | system_error (C++11) |

#### 工具
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<utility>` | 工具类 | pair, swap, move, forward |
| `<tuple>` | 元组 | tuple (C++11) |
| `<variant>` | 类型安全的联合 | variant (C++17) |
| `<optional>` | 可选值 | optional (C++17) |
| `<any>` | 任意类型 | any (C++17) |
| `<bitset>` | 位集合 | bitset |

#### 智能指针
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<memory>` | 内存管理 | unique_ptr, shared_ptr, weak_ptr |

#### 并发
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<thread>` | 线程 | thread (C++11) |
| `<mutex>` | 互斥锁 | mutex, lock_guard (C++11) |
| `<condition_variable>` | 条件变量 | condition_variable (C++11) |
| `<future>` | 异步操作 | promise, future (C++11) |
| `<atomic>` | 原子操作 | atomic (C++11) |

#### 类型特性
| 头文件 | 用途 | 关键功能 |
|--------|------|---------|
| `<type_traits>` | 类型特性 | is_same, remove_cv (C++11) |
| `<typeinfo>` | 类型信息 | typeid, type_info |

#### 算法库
| 头文件 | 用途 | 关键算法 |
|--------|------|---------|
| `<algorithm>` | 通用算法 | sort, find, transform |
| `<random>` | 随机数 | mt19937, uniform_int_distribution (C++11) |

#### 数组
| 头文件 | 用途 | 关键类型 |
|--------|------|---------|
| `<array>` | 固定大小数组 | array (C++11) |
| `<span>` | 数组视图 | span (C++20) |

### C++ 标准库详细说明

#### 1. 容器库

**序列容器**

| 容器 | 特点 | 时间复杂度 | 适用场景 |
|------|------|----------|---------|
| `vector` | 动态数组 | O(1) 访问，O(1) 尾部插入 | 通用序列 |
| `deque` | 双端队列 | O(1) 两端插入 | 需要两端操作 |
| `list` | 双向链表 | O(1) 任意位置插入 | 频繁中间插入 |
| `forward_list` | 单向链表 | O(1) 任意位置插入 | 节省空间 |

**关联容器**

| 容器 | 特点 | 时间复杂度 | 适用场景 |
|------|------|----------|---------|
| `set` | 有序集合 | O(log n) | 需要排序 |
| `map` | 有序映射 | O(log n) | 键值对，需要排序 |
| `multiset` | 允许重复 | O(log n) | 需要重复键 |
| `multimap` | 允许重复键 | O(log n) | 需要重复键 |
| `unordered_set` | 哈希集合 | 平均 O(1) | 高性能查找 |
| `unordered_map` | 哈希映射 | 平均 O(1) | 高性能映射 |

**容器适配器**

| 适配器 | 底层容器 | 特点 |
|--------|---------|------|
| `stack` | deque/vector | LIFO |
| `queue` | deque/list | FIFO |
| `priority_queue` | vector | 优先级队列 |

#### 2. 迭代器

**迭代器类别**

| 类别 | 特点 | 示例 |
|------|------|------|
| 输入迭代器 | 只读单向 | istream_iterator |
| 输出迭代器 | 只写单向 | ostream_iterator |
| 前向迭代器 | 可读写，单向 | list, set |
| 双向迭代器 | 可读写，双向 | map, set |
| 随机访问迭代器 | 可跳跃访问 | vector, array, deque |

**迭代器操作**

| 操作 | 说明 |
|------|------|
| `*it` | 解引用 |
| `it->member` | 成员访问 |
| `++it, it++` | 前进 |
| `--it, it--` | 后退（双向） |
| `it + n, it - n` | 随机访问 |
| `it1 - it2` | 距离（随机访问） |
| `it1 < it2` | 比较（随机访问） |

#### 3. 算法库

**非修改算法**

| 算法 | 功能 | 用途 |
|------|------|------|
| `find` | 查找元素 | 搜索 |
| `find_if` | 按条件查找 | 条件搜索 |
| `count` | 计数 | 统计 |
| `for_each` | 遍历处理 | 执行操作 |
| `equal` | 比较相等 | 容器比较 |
| `mismatch` | 查找不匹配 | 比较差异 |
| `search` | 搜索子序列 | 序列匹配 |

**修改算法**

| 算法 | 功能 | 用途 |
|------|------|------|
| `copy` | 复制 | 数据迁移 |
| `move` | 移动 | 高效迁移 |
| `transform` | 变换 | 数据转换 |
| `fill` | 填充 | 初始化 |
| `generate` | 生成 | 填充数据 |
| `replace` | 替换 | 数据修改 |
| `remove` | 移除 | 清理数据 |
| `reverse` | 反转 | 顺序调整 |
| `rotate` | 旋转 | 循环移位 |
| `shuffle` | 打乱 | 随机化 |
| `unique` | 去重 | 移除重复 |

**排序算法**

| 算法 | 功能 | 复杂度 |
|------|------|--------|
| `sort` | 排序 | 平均 O(n log n) |
| `stable_sort` | 稳定排序 | O(n log n) |
| `partial_sort` | 部分排序 | O(n log m) |
| `nth_element` | 第 n 元素 | 平均 O(n) |

**二分搜索算法**

| 算法 | 功能 | 要求 |
|------|------|------|
| `lower_bound` | 第一个 ≥ | 已排序 |
| `upper_bound` | 第一个 > | 已排序 |
| `binary_search` | 查找 | 已排序 |
| `equal_range` | 范围 | 已排序 |

**数值算法**

| 算法 | 功能 | 示例 |
|------|------|------|
| `accumulate` | 累加 | 求和 |
| `inner_product` | 内积 | 点积 |
| `partial_sum` | 部分和 | 前缀和 |
| `adjacent_difference` | 相邻差 | 差分 |

#### 4. 字符串库

**string 类**

| 成员函数 | 功能 |
|----------|------|
| `length()`, `size()` | 长度 |
| `empty()` | 是否为空 |
| `substr()` | 子字符串 |
| `find()`, `rfind()` | 查找 |
| `replace()` | 替换 |
| `insert()` | 插入 |
| `erase()` | 删除 |
| `append()`, `push_back()` | 追加 |
| `compare()` | 比较 |
| `c_str()` | C 字符串 |

#### 5. 智能指针

**智能指针类型**

| 类型 | 特点 | 用途 |
|------|------|------|
| `unique_ptr` | 独占所有权 | 自动释放 |
| `shared_ptr` | 共享所有权 | 多处共享 |
| `weak_ptr` | 弱引用 | 避免循环 |

**创建方式**

| 方法 | 示例 |
|------|------|
| 直接构造 | `unique_ptr<T> ptr(new T)` |
| make_unique | `auto ptr = make_unique<T>()` |
| make_shared | `auto ptr = make_shared<T>()` |

#### 6. 并发库

**线程**

| 函数 | 功能 |
|------|------|
| `thread()` | 创建线程 |
| `join()` | 等待结束 |
| `detach()` | 分离 |
| `hardware_concurrency()` | 线程数 |

**互斥锁**

| 类型 | 特点 |
|------|------|
| `mutex` | 基本互斥锁 |
| `recursive_mutex` | 可重入 |
| `timed_mutex` | 超时 |
| `shared_mutex` | 读写锁 (C++17) |

**锁封装**

| 类型 | 特点 |
|------|------|
| `lock_guard` | RAII，简单 |
| `unique_lock` | RAII，灵活 |
| `scoped_lock` | 多锁 (C++17) |
| `shared_lock` | 共享锁 (C++14) |

## 版本差异

### C 标准版本

| 版本 | 年份 | 主要特性 |
|------|------|---------|
| C89/C90 | 1989/1990 | ANSI/ISO 标准 |
| C99 | 1999 | bool, long long, 变长数组 |
| C11 | 2011 | 线程, 原子操作, 泛型选择 |
| C17 | 2018 | C11 的修正 |
| C23 | 2023 | 新特性（预览） |

### C++ 标准版本

| 版本 | 年份 | 主要特性 |
|------|------|---------|
| C++98 | 1998 | 首个标准 |
| C++03 | 2003 | 修正版本 |
| C++11 | 2011 | Lambda, 智能指针, 右值引用, auto |
| C++14 | 2014 | 泛型 lambda, make_unique |
| C++17 | 2017 | 文件系统, string_view, optional |
| C++20 | 2020 | 模块, 协程, 概念, ranges |
| C++23 | 2023 | 新特性（预览） |

### 主要版本差异

**C++11 引入**

- `auto` 类型推导
- Lambda 表达式
- 右值引用与移动语义
- 智能指针 (`unique_ptr`, `shared_ptr`)
- 线程库 (`thread`, `mutex`, `condition_variable`)
- 原子操作 (`atomic`)
- 随机数库 (`<random>`)
- 正则表达式 (`<regex>`)
- `tuple`
- `function`, `bind`
- `unordered_map`, `unordered_set`

**C++14 引入**

- 泛型 lambda
- `make_unique`
- 二进制字面量 (`0b1010`)
- 数位分隔符 (`1'000'000`)
- `std::shared_mutex`

**C++17 引入**

- `std::filesystem`
- `std::string_view`
- `std::optional`, `std::variant`, `std::any`
- 结构化绑定
- `if constexpr`
- `std::shared_mutex`
- 并行算法
- 折叠表达式
- `std::jthread`

**C++20 引入**

- 模块 (`module`)
- 协程 (`co_await`, `co_yield`, `co_return`)
- 概念 (`concept`)
- Ranges 库 (`std::ranges`)
- 三路比较运算符 (`<=>`)
- `std::span`
- 格式化库 (`std::format`)
- 常量求值 (`consteval`)
- `std::bit`

## 最佳实践

### C 语言

1. **内存管理**
   - 始终检查 `malloc` 返回值
   - 成对使用 `malloc`/`free`
   - 使用 `valgrind` 检测内存泄漏

2. **字符串操作**
   - 优先使用 `strncpy` 而非 `strcpy`
   - 确保字符串以 `\0` 结尾
   - 避免缓冲区溢出

3. **错误处理**
   - 检查所有函数返回值
   - 使用 `errno` 和 `perror`
   - 合理使用断言 `assert`

4. **可移植性**
   - 使用标准整数类型 (`<stdint.h>`)
   - 注意字节序问题
   - 避免未定义行为

### C++ 语言

1. **内存管理**
   - 优先使用智能指针
   - 使用 `make_shared`, `make_unique`
   - 避免 `new`/`delete`

2. **容器选择**
   - 优先使用 `vector`
   - 需要排序用 `map`/`set`
   - 需要高性能用 `unordered_map`/`unordered_set`

3. **性能优化**
   - 使用移动语义
   - 优先使用引用传递
   - 使用 `emplace_back` 代替 `push_back`

4. **现代 C++**
   - 使用 `auto` 简化代码
   - 使用 Lambda 替代函数对象
   - 使用范围 for 循环

5. **并发编程**
   - 优先使用高层次的同步原语
   - 避免数据竞争
   - 使用 RAII 管理锁

### 通用建议

1. **代码风格**
   - 遵循命名规范
   - 添加适当注释
   - 保持函数简洁

2. **错误处理**
   - 使用异常（C++）
   - 检查返回值（C）
   - 提供错误信息

3. **测试**
   - 编写单元测试
   - 边界条件测试
   - 使用静态分析工具

4. **文档**
   - 使用 Doxygen
   - 说明函数参数和返回值
   - 提供使用示例
