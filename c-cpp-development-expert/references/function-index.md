# C/C++ 函数库索引

## 目录
- [C 标准库函数索引](#c-标准库函数索引)
- [C++ 标准库函数索引](#c-标准库函数索引-1)
- [常用第三方库](#常用第三方库)
- [函数使用示例](#函数使用示例)

## C 标准库函数索引

### `<stdio.h>` - 标准输入输出

#### 文件操作
```c
// 文件打开与关闭
FILE* fopen(const char* filename, const char* mode);
int fclose(FILE* stream);

// 临时文件
FILE* tmpfile(void);
char* tmpnam(char* s);

// 文件刷新
int fflush(FILE* stream);

// 文件删除与重命名
int remove(const char* filename);
int rename(const char* oldname, const char* newname);

// 文件位置
int fseek(FILE* stream, long offset, int origin);
long ftell(FILE* stream);
void rewind(FILE* stream);
int fgetpos(FILE* stream, fpos_t* pos);
int fsetpos(FILE* stream, const fpos_t* pos);
```

#### 格式化输入输出
```c
// 输出
int printf(const char* format, ...);
int fprintf(FILE* stream, const char* format, ...);
int sprintf(char* str, const char* format, ...);
int snprintf(char* str, size_t size, const char* format, ...);

// 输入
int scanf(const char* format, ...);
int fscanf(FILE* stream, const char* format, ...);
int sscanf(const char* str, const char* format, ...);
```

#### 字符输入输出
```c
// 字符输出
int fputc(int c, FILE* stream);
int putc(int c, FILE* stream);
int putchar(int c);
int fputs(const char* s, FILE* stream);
int puts(const char* s);

// 字符输入
int fgetc(FILE* stream);
int getc(FILE* stream);
int getchar(void);
char* fgets(char* s, int n, FILE* stream);
```

#### 块输入输出
```c
size_t fread(void* ptr, size_t size, size_t nmemb, FILE* stream);
size_t fwrite(const void* ptr, size_t size, size_t nmemb, FILE* stream);
```

#### 文件状态
```c
int feof(FILE* stream);
int ferror(FILE* stream);
void clearerr(FILE* stream);
```

### `<stdlib.h>` - 通用工具函数

#### 内存管理
```c
void* malloc(size_t size);
void* calloc(size_t nmemb, size_t size);
void* realloc(void* ptr, size_t size);
void free(void* ptr);
```

#### 进程控制
```c
// 程序终止
void abort(void);
void exit(int status);
void _Exit(int status);

// 环境变量
char* getenv(const char* name);
int putenv(char* string);
int setenv(const char* name, const char* value, int overwrite);
int unsetenv(const char* name);

// 系统命令
int system(const char* string);
```

#### 类型转换
```c
// 字符串转数字
double atof(const char* str);
int atoi(const char* str);
long atol(const char* str);
long long atoll(const char* str);

// 更安全的转换
double strtod(const char* str, char** endptr);
float strtof(const char* str, char** endptr);
long strtol(const char* str, char** endptr, int base);
unsigned long strtoul(const char* str, char** endptr, int base);
```

#### 排序与搜索
```c
void qsort(void* base, size_t nmemb, size_t size,
           int (*compar)(const void*, const void*));
void* bsearch(const void* key, const void* base,
              size_t nmemb, size_t size,
              int (*compar)(const void*, const void*));
```

#### 随机数
```c
int rand(void);
void srand(unsigned int seed);
```

#### 绝对值
```c
int abs(int j);
long labs(long j);
long long llabs(long long j);
```

#### 整数除法
```c
div_t div(int numer, int denom);
ldiv_t ldiv(long numer, long denom);
lldiv_t lldiv(long long numer, long long denom);
```

### `<string.h>` - 字符串操作

#### 字符串拷贝
```c
char* strcpy(char* dest, const char* src);
char* strncpy(char* dest, const char* src, size_t n);
char* strdup(const char* s);  // POSIX
```

#### 字符串连接
```c
char* strcat(char* dest, const char* src);
char* strncat(char* dest, const char* src, size_t n);
```

#### 字符串比较
```c
int strcmp(const char* s1, const char* s2);
int strncmp(const char* s1, const char* s2, size_t n);
int strcoll(const char* s1, const char* s2);
size_t strxfrm(char* dest, const char* src, size_t n);
```

#### 字符串搜索
```c
char* strchr(const char* s, int c);
char* strrchr(const char* s, int c);
size_t strcspn(const char* s, const char* reject);
char* strpbrk(const char* s, const char* accept);
size_t strspn(const char* s, const char* accept);
char* strstr(const char* haystack, const char* needle);
char* strtok(char* str, const char* delim);
```

#### 字符串长度
```c
size_t strlen(const char* s);
```

#### 字符串错误
```c
char* strerror(int errnum);
int strerror_r(int errnum, char* buf, size_t buflen);
```

#### 内存操作
```c
void* memcpy(void* dest, const void* src, size_t n);
void* memmove(void* dest, const void* src, size_t n);
void* memset(void* s, int c, size_t n);
int memcmp(const void* s1, const void* s2, size_t n);
void* memchr(const void* s, int c, size_t n);
```

### `<math.h>` - 数学函数

#### 基础数学
```c
double sin(double x);
double cos(double x);
double tan(double x);
double asin(double x);
double acos(double x);
double atan(double x);
double atan2(double y, double x);

double sinh(double x);
double cosh(double x);
double tanh(double x);
double asinh(double x);
double acosh(double x);
double atanh(double x);
```

#### 指数与对数
```c
double exp(double x);
double exp2(double x);
double expm1(double x);
double log(double x);
double log10(double x);
double log2(double x);
double log1p(double x);
```

#### 幂函数
```c
double pow(double x, double y);
double sqrt(double x);
double cbrt(double x);
double hypot(double x, double y);
```

#### 误差函数
```c
double erf(double x);
double erfc(double x);
```

#### 伽马函数
```c
double tgamma(double x);
double lgamma(double x);
```

#### 取整函数
```c
double ceil(double x);
double floor(double x);
double trunc(double x);
double round(double x);
long lround(double x);
long long llround(double x);

double rint(double x);
long lrint(double x);
long long llrint(double x);

double nearbyint(double x);
```

#### 取余函数
```c
double fmod(double x, double y);
double remainder(double x, double y);
double remquo(double x, double y, int* quo);
```

#### 其他函数
```c
double fabs(double x);
double copysign(double x, double y);
double fdim(double x, double y);
double fmax(double x, double y);
double fmin(double x, double y);
double fma(double x, double y, double z);

double nan(const char* tagp);
double nanf(const char* tagp);
double nanl(const char* tagp);
```

#### 分类与比较
```c
int fpclassify(double x);
int isfinite(double x);
int isinf(double x);
int isnan(double x);
int isnormal(double x);
int signbit(double x);

int isgreater(double x, double y);
int isgreaterequal(double x, double y);
int isless(double x, double y);
int islessequal(double x, double y);
int islessgreater(double x, double y);
int isunordered(double x, double y);
```

### `<ctype.h>` - 字符分类

#### 字符分类
```c
int isalnum(int c);
int isalpha(int c);
int iscntrl(int c);
int isdigit(int c);
int isgraph(int c);
int islower(int c);
int isprint(int c);
int ispunct(int c);
int isspace(int c);
int isupper(int c);
int isxdigit(int c);
```

#### 字符转换
```c
int tolower(int c);
int toupper(int c);
```

### `<time.h>` - 时间与日期

#### 时间处理
```c
time_t time(time_t* tloc);
double difftime(time_t time1, time_t time0);
time_t mktime(struct tm* timeptr);
```

#### 时间转换
```c
char* asctime(const struct tm* timeptr);
char* ctime(const time_t* timer);
struct tm* gmtime(const time_t* timer);
struct tm* localtime(const time_t* timer);

size_t strftime(char* s, size_t max, const char* format,
                const struct tm* timeptr);
char* strptime(const char* s, const char* format, struct tm* tm);
```

#### 时钟
```c
clock_t clock(void);
```

### `<assert.h>` - 断言

```c
void assert(scalar expression);
```

### `<errno.h>` - 错误码

```c
extern int errno;
```

### `<stddef.h>` - 标准定义

```c
typedef ptrdiff_t;  // 指针差值类型
typedef size_t;     // 无符号大小类型
typedef wchar_t;    // 宽字符类型

#define NULL ((void*)0)
#define offsetof(type, member) /* 宏定义 */
```

### `<stdbool.h>` - 布尔类型 (C99)

```c
#define bool _Bool
#define true 1
#define false 0
```

### `<stdint.h>` - 标准整数类型 (C99)

#### 精确宽度整数类型
```c
int8_t, uint8_t
int16_t, uint16_t
int32_t, uint32_t
int64_t, uint64_t
```

#### 最小宽度整数类型
```c
int_least8_t, uint_least8_t
int_least16_t, uint_least16_t
int_least32_t, uint_least32_t
int_least64_t, uint_least64_t
```

#### 最快最小宽度整数类型
```c
int_fast8_t, uint_fast8_t
int_fast16_t, uint_fast16_t
int_fast32_t, uint_fast32_t
int_fast64_t, uint_fast64_t
```

#### 指针类型
```c
intptr_t, uintptr_t
```

#### 最大整数类型
```c
intmax_t, uintmax_t
```

#### 常量宏
```c
INT8_MIN, INT8_MAX, UINT8_MAX
INT16_MIN, INT16_MAX, UINT16_MAX
INT32_MIN, INT32_MAX, UINT32_MAX
INT64_MIN, INT64_MAX, UINT64_MAX
INTMAX_MIN, INTMAX_MAX, UINTMAX_MAX
```

### `<threads.h>` - 线程支持 (C11)

```c
// 线程管理
typedef thrd_t;  // 线程标识符
int thrd_create(thrd_t* thr, thrd_start_t func, void* arg);
int thrd_detach(thrd_t thr);
int thrd_equal(thrd_t thr0, thrd_t thr1);
thrd_t thrd_current(void);
int thrd_sleep(const struct timespec* duration, struct timespec* remaining);
void thrd_yield(void);
int thrd_exit(int res);
int thrd_join(thrd_t thr, int* res);

// 互斥锁
typedef mtx_t;  // 互斥锁
int mtx_init(mtx_t* mutex, int type);
int mtx_lock(mtx_t* mutex);
int mtx_timedlock(mtx_t* mutex, const struct timespec* time_point);
int mtx_trylock(mtx_t* mutex);
int mtx_unlock(mtx_t* mutex);
void mtx_destroy(mtx_t* mutex);

// 条件变量
typedef cnd_t;  // 条件变量
int cnd_init(cnd_t* cond);
int cnd_signal(cnd_t* cond);
int cnd_broadcast(cnd_t* cond);
int cnd_wait(cnd_t* cond, mtx_t* mutex);
int cnd_timedwait(cnd_t* cond, mtx_t* mutex,
                 const struct timespec* time_point);
void cnd_destroy(cnd_t* cond);
```

### `<stdatomic.h>` - 原子操作 (C11)

```c
// 原子类型
typedef atomic_bool;    // _Atomic _Bool
typedef atomic_char;    // _Atomic char
typedef atomic_int;     // _Atomic int
// ... 等等

// 初始化
#define ATOMIC_VAR_INIT(value)
#define atomic_init(obj, value)

// 操作
#define atomic_is_lock_free(obj)
#define atomic_store(obj, desired)
#define atomic_store_explicit(obj, desired, order)
#define atomic_load(obj)
#define atomic_load_explicit(obj, order)
#define atomic_exchange(obj, desired)
#define atomic_exchange_explicit(obj, desired, order)
#define atomic_compare_exchange_strong(obj, expected, desired)
#define atomic_compare_exchange_strong_explicit(obj, expected, desired, success, failure)
#define atomic_compare_exchange_weak(obj, expected, desired)
#define atomic_compare_exchange_weak_explicit(obj, expected, desired, success, failure)

#define atomic_fetch_add(obj, arg)
#define atomic_fetch_add_explicit(obj, arg, order)
// ... fetch_sub, fetch_or, fetch_xor, fetch_and

#define atomic_flag
#define ATOMIC_FLAG_INIT
#define atomic_flag_test_and_set(obj)
#define atomic_flag_test_and_set_explicit(obj, order)
#define atomic_flag_clear(obj)
#define atomic_flag_clear_explicit(obj, order)
```

## C++ 标准库函数索引

### `<iostream>` - 输入输出流

#### 标准流对象
```cpp
std::cin;   // 标准输入
std::cout;  // 标准输出
std::cerr;  // 标准错误（无缓冲）
std::clog;  // 标准错误（有缓冲）
```

#### 输出操作符
```cpp
std::ostream& operator<<(std::ostream& os, const T& value);
```

#### 输入操作符
```cpp
std::istream& operator>>(std::istream& is, T& value);
```

#### 成员函数
```cpp
// 格式化
std::ios_base& flags(std::ios_base::fmtflags newflags);
std::ios_base& setf(std::ios_base::fmtflags flags);
std::ios_base& unsetf(std::ios_base::fmtflags flags);
std::streamsize precision() const;
std::streamsize precision(std::streamsize newprec);
std::streamsize width() const;
std::streamsize width(std::streamsize newwidth);

// 状态
bool good() const;
bool eof() const;
bool fail() const;
bool bad() const;
operator bool() const;
bool operator!() const;
iostate rdstate() const;
void clear(iostate state = goodbit);
void setstate(iostate state);

// 定位
pos_type tellg();
pos_type tellp();
istream& seekg(pos_type pos);
istream& seekg(off_type off, ios_base::seekdir dir);
ostream& seekp(pos_type pos);
ostream& seekp(off_type off, ios_base::seekdir dir);

// 同步
int sync();

// 字符输入输出
int_type get();
istream& get(char_type& c);
istream& get(char_type* s, streamsize n);
istream& get(char_type* s, streamsize n, char_type delim);
istream& get(streambuf& sb);
istream& get(streambuf& sb, char_type delim);

istream& getline(char_type* s, streamsize n);
istream& getline(char_type* s, streamsize n, char_type delim);

istream& ignore(streamsize n = 1, int_type delim = EOF);

int_type peek();
int_type putback(char_type c);
istream& unget();

ostream& put(char_type c);
ostream& write(const char_type* s, streamsize n);

// 刷新
ostream& flush();
```

### `<fstream>` - 文件流

```cpp
// 文件输入流
std::ifstream(const char* filename, 
             ios_base::openmode mode = ios_base::in);
void open(const char* filename,
          ios_base::openmode mode = ios_base::in);
bool is_open() const;
void close();

// 文件输出流
std::ofstream(const char* filename,
             ios_base::openmode mode = ios_base::out);
void open(const char* filename,
          ios_base::openmode mode = ios_base::out);
bool is_open() const;
void close();

// 文件输入输出流
std::fstream(const char* filename,
            ios_base::openmode mode = ios_base::in | ios_base::out);
void open(const char* filename,
          ios_base::openmode mode = ios_base::in | ios_base::out);
bool is_open() const;
void close();
```

### `<sstream>` - 字符串流

```cpp
// 字符串输入流
std::istringstream(const std::string& str);
void str(const std::string& s);
std::string str() const;

// 字符串输出流
std::ostringstream();
std::ostringstream(const std::string& str);
void str(const std::string& s);
std::string str() const;

// 字符串输入输出流
std::stringstream();
std::stringstream(const std::string& str);
void str(const std::string& s);
std::string str() const;
```

### `<string>` - 字符串类

#### 构造函数
```cpp
std::string();
std::string(size_t count, char ch);
std::string(const std::string& other, size_t pos, size_t count = npos);
std::string(const char* s, size_t count);
std::string(const char* s);
template <class InputIt>
std::string(InputIt first, InputIt last);
std::string(std::initializer_list<char> ilist);
std::string(const std::string& other);
std::string(std::string&& other) noexcept;
```

#### 赋值
```cpp
std::string& operator=(const std::string& other);
std::string& operator=(std::string&& other) noexcept;
std::string& operator=(const char* s);
std::string& operator=(char ch);
std::string& operator=(std::initializer_list<char> ilist);

void assign(size_t count, char ch);
void assign(const std::string& other);
void assign(const std::string& other, size_t pos, size_t count);
void assign(const char* s, size_t count);
void assign(const char* s);
template <class InputIt>
void assign(InputIt first, InputIt last);
void assign(std::initializer_list<char> ilist);
```

#### 元素访问
```cpp
reference operator[](size_type pos);
const_reference operator[](size_type pos) const;
reference at(size_type pos);
const_reference at(size_type pos) const;
reference front();
const_reference front() const;
reference back();
const_reference back() const;
const char* data() const noexcept;
const char* c_str() const noexcept;
operator std::string_view() const noexcept;
```

#### 迭代器
```cpp
iterator begin() noexcept;
const_iterator begin() const noexcept;
const_iterator cbegin() const noexcept;

iterator end() noexcept;
const_iterator end() const noexcept;
const_iterator cend() const noexcept;

reverse_iterator rbegin() noexcept;
const_reverse_iterator rbegin() const noexcept;
const_reverse_iterator crbegin() const noexcept;

reverse_iterator rend() noexcept;
const_reverse_iterator rend() const noexcept;
const_reverse_iterator crend() const noexcept;
```

#### 容量
```cpp
bool empty() const noexcept;
size_type size() const noexcept;
size_type length() const noexcept;
size_type max_size() const noexcept;
void reserve(size_type new_cap = 0);
size_type capacity() const noexcept;
void shrink_to_fit();
```

#### 修改器
```cpp
void clear() noexcept;
std::string& insert(size_type pos, size_type count, char ch);
std::string& insert(size_type pos, const char* s);
std::string& insert(size_type pos, const std::string& str);
std::string& insert(size_type pos, const std::string& str,
                    size_type str_pos, size_type str_count = npos);
iterator insert(const_iterator pos, char ch);
iterator insert(const_iterator pos, size_type count, char ch);
template <class InputIt>
iterator insert(const_iterator pos, InputIt first, InputIt last);
iterator insert(const_iterator pos, std::initializer_list<char> ilist);

std::string& erase(size_type pos = 0, size_type count = npos);
iterator erase(const_iterator position);
iterator erase(const_iterator first, const_iterator last);

void push_back(char ch);
void pop_back();

std::string& append(size_type count, char ch);
std::string& append(const std::string& str);
std::string& append(const std::string& str,
                    size_type pos, size_type count = npos);
std::string& append(const char* s, size_type count);
std::string& append(const char* s);
template <class InputIt>
std::string& append(InputIt first, InputIt last);
std::string& append(std::initializer_list<char> ilist);

std::string& operator+=(const std::string& str);
std::string& operator+=(char ch);
std::string& operator+=(const char* s);
std::string& operator+=(std::initializer_list<char> ilist);

int compare(const std::string& other) const noexcept;
int compare(size_type pos1, size_type count1,
            const std::string& other) const;
int compare(size_type pos1, size_type count1,
            const std::string& other,
            size_type pos2, size_type count2 = npos) const;
int compare(const char* s) const;
int compare(size_type pos1, size_type count1,
            const char* s) const;
int compare(size_type pos1, size_type count1,
            const char* s, size_type count2) const;

std::string& replace(size_type pos, size_type count,
                      const std::string& str);
std::string& replace(const_iterator first, const_iterator last,
                      const std::string& str);
std::string& replace(size_type pos, size_type count,
                      const std::string& str,
                      size_type pos2, size_type count2 = npos);
std::string& replace(size_type pos, size_type count,
                      const char* s, size_type count2);

std::string substr(size_type pos = 0, size_type count = npos) const;

size_type copy(char* dest, size_type count, size_type pos = 0) const;

void resize(size_type count);
void resize(size_type count, char ch);

void swap(std::string& other) noexcept;
```

#### 查找
```cpp
size_type find(const std::string& str, size_type pos = 0) const noexcept;
size_type find(const char* s, size_type pos, size_type count) const;
size_type find(const char* s, size_type pos = 0) const;
size_type find(char ch, size_type pos = 0) const noexcept;

size_type rfind(const std::string& str, size_type pos = npos) const noexcept;
size_type rfind(const char* s, size_type pos, size_type count) const;
size_type rfind(const char* s, size_type pos = npos) const;
size_type rfind(char ch, size_type pos = npos) const noexcept;

size_type find_first_of(const std::string& str, size_type pos = 0) const noexcept;
size_type find_first_not_of(const std::string& str, size_type pos = 0) const noexcept;
size_type find_last_of(const std::string& str, size_type pos = npos) const noexcept;
size_type find_last_not_of(const std::string& str, size_type pos = npos) const noexcept;
```

#### 非成员函数
```cpp
// 连接
std::string operator+(const std::string& lhs, const std::string& rhs);
std::string operator+(std::string&& lhs, const std::string& rhs);
std::string operator+(const std::string& lhs, std::string&& rhs);
std::string operator+(std::string&& lhs, std::string&& rhs);
std::string operator+(const char* lhs, const std::string& rhs);
std::string operator+(std::string&& lhs, const char* rhs);
std::string operator+(char lhs, const std::string& rhs);
std::string operator+(std::string&& lhs, char rhs);

// 比较
bool operator==(const std::string& lhs, const std::string& rhs) noexcept;
bool operator!=(const std::string& lhs, const std::string& rhs) noexcept;
bool operator<(const std::string& lhs, const std::string& rhs) noexcept;
bool operator<=(const std::string& lhs, const std::string& rhs) noexcept;
bool operator>(const std::string& lhs, const std::string& rhs) noexcept;
bool operator>=(const std::string& lhs, const std::string& rhs) noexcept;

// 输入输出
std::istream& operator>>(std::istream& is, std::string& str);
std::ostream& operator<<(std::ostream& os, const std::string& str);
std::istream& getline(std::istream& is, std::string& str, char delim);
std::istream& getline(std::istream&& is, std::string& str, char delim);
std::istream& getline(std::istream& is, std::string& str);
std::istream& getline(std::istream&& is, std::string& str);

// 交换
void swap(std::string& lhs, std::string& rhs) noexcept;

// 转换
std::string to_string(int value);
std::string to_string(long value);
std::string to_string(long long value);
std::string to_string(unsigned value);
std::string to_string(unsigned long value);
std::string to_string(unsigned long long value);
std::string to_string(float value);
std::string to_string(double value);
std::string to_string(long double value);

int stoi(const std::string& str, size_t* pos = 0, int base = 10);
long stol(const std::string& str, size_t* pos = 0, int base = 10);
unsigned long stoul(const std::string& str, size_t* pos = 0, int base = 10);
long long stoll(const std::string& str, size_t* pos = 0, int base = 10);
unsigned long long stoull(const std::string& str, size_t* pos = 0, int base = 10);

float stof(const std::string& str, size_t* pos = 0);
double stod(const std::string& str, size_t* pos = 0);
long double stold(const std::string& str, size_t* pos = 0);
```

### `<vector>` - 动态数组

#### 构造函数
```cpp
std::vector();
explicit std::vector(const Allocator& alloc);
explicit std::vector(size_type count, const T& value = T(),
                     const Allocator& alloc = Allocator());
explicit std::vector(size_type count, const Allocator& alloc = Allocator());
template <class InputIt>
std::vector(InputIt first, InputIt last,
            const Allocator& alloc = Allocator());
std::vector(const std::vector& other);
std::vector(const std::vector& other, const Allocator& alloc);
std::vector(std::vector&& other) noexcept;
std::vector(std::vector&& other, const Allocator& alloc);
std::vector(std::initializer_list<T> init,
            const Allocator& alloc = Allocator());
```

#### 赋值
```cpp
std::vector& operator=(const std::vector& other);
std::vector& operator=(std::vector&& other) noexcept;
std::vector& operator=(std::initializer_list<T> ilist);

void assign(size_type count, const T& value);
template <class InputIt>
void assign(InputIt first, InputIt last);
void assign(std::initializer_list<T> ilist);
```

#### 元素访问
```cpp
reference operator[](size_type pos);
const_reference operator[](size_type pos) const;
reference at(size_type pos);
const_reference at(size_type pos) const;
reference front();
const_reference front() const;
reference back();
const_reference back() const;
T* data() noexcept;
const T* data() const noexcept;
```

#### 迭代器
```cpp
iterator begin() noexcept;
const_iterator begin() const noexcept;
const_iterator cbegin() const noexcept;

iterator end() noexcept;
const_iterator end() const noexcept;
const_iterator cend() const noexcept;

reverse_iterator rbegin() noexcept;
const_reverse_iterator rbegin() const noexcept;
const_reverse_iterator crbegin() const noexcept;

reverse_iterator rend() noexcept;
const_reverse_iterator rend() const noexcept;
const_reverse_iterator crend() const noexcept;
```

#### 容量
```cpp
bool empty() const noexcept;
size_type size() const noexcept;
size_type max_size() const noexcept;
void reserve(size_type new_cap);
size_type capacity() const noexcept;
void shrink_to_fit();
```

#### 修改器
```cpp
void clear() noexcept;

iterator insert(const_iterator pos, const T& value);
iterator insert(const_iterator pos, T&& value);
iterator insert(const_iterator pos, size_type count, const T& value);
template <class InputIt>
iterator insert(const_iterator pos, InputIt first, InputIt last);
iterator insert(const_iterator pos, std::initializer_list<T> ilist);

template <class... Args>
iterator emplace(const_iterator pos, Args&&... args);

template <class... Args>
reference emplace_back(Args&&... args);
void push_back(const T& value);
void push_back(T&& value);

iterator erase(const_iterator pos);
iterator erase(const_iterator first, const_iterator last);

void pop_back();

void resize(size_type count);
void resize(size_type count, const T& value);

void swap(std::vector& other) noexcept;
```

### `<map>` - 关联容器

```cpp
// 构造函数
std::map();
explicit std::map(const Compare& comp, const Allocator& alloc = Allocator());
template <class InputIt>
std::map(InputIt first, InputIt last,
         const Compare& comp = Allocator(),
         const Allocator& alloc = Allocator());
std::map(const std::map& other);
std::map(std::map&& other) noexcept;
std::map(std::initializer_list<value_type> init,
         const Compare& comp = Allocator(),
         const Allocator& alloc = Allocator());

// 元素访问
T& operator[](const Key& key);
T& operator[](Key&& key);
T& at(const Key& key);
const T& at(const Key& key) const;

// 迭代器
iterator begin() noexcept;
const_iterator begin() const noexcept;
const_iterator cbegin() const noexcept;

iterator end() noexcept;
const_iterator end() const noexcept;
const_iterator cend() const noexcept;

// 容量
bool empty() const noexcept;
size_type size() const noexcept;
size_type max_size() const noexcept;

// 修改器
void clear() noexcept;

std::pair<iterator, bool> insert(const value_type& value);
template <class P>
std::pair<iterator, bool> insert(P&& value);
iterator insert(const_iterator hint, const value_type& value);
template <class P>
iterator insert(const_iterator hint, P&& value);
template <class InputIt>
void insert(InputIt first, InputIt last);
void insert(std::initializer_list<value_type> ilist);

template <class... Args>
std::pair<iterator, bool> emplace(Args&&... args);
template <class... Args>
iterator emplace_hint(const_iterator hint, Args&&... args);

iterator erase(const_iterator pos);
iterator erase(const_iterator first, const_iterator last);
size_type erase(const key_type& key);

void swap(std::map& other) noexcept;

// 查找
iterator find(const Key& key);
const_iterator find(const Key& key) const;
template <class K>
iterator find(const K& x);
template <class K>
const_iterator find(const K& x) const;

size_type count(const Key& key) const;
template <class K>
size_type count(const K& x) const;

iterator lower_bound(const Key& key);
const_iterator lower_bound(const Key& key) const;
template <class K>
iterator lower_bound(const K& x);
template <class K>
const_iterator lower_bound(const K& x) const;

iterator upper_bound(const Key& key);
const_iterator upper_bound(const Key& key) const;
template <class K>
iterator upper_bound(const K& x);
template <class K>
const_iterator upper_bound(const K& x) const;

std::pair<iterator, iterator> equal_range(const Key& key);
std::pair<const_iterator, const_iterator> equal_range(const Key& key) const;
template <class K>
std::pair<iterator, iterator> equal_range(const K& x);
template <class K>
std::pair<const_iterator, const_iterator> equal_range(const K& x) const;
```

### `<unordered_map>` - 无序关联容器 (C++11)

```cpp
// 构造函数
explicit std::unordered_map(size_type bucket_count = /* implementation-defined */,
                         const Hash& hash = Hash(),
                         const key_equal& equal = key_equal(),
                         const Allocator& alloc = Allocator());
std::unordered_map(size_type bucket_count,
                  const Allocator& alloc);
std::unordered_map(size_type bucket_count,
                  const Hash& hash,
                  const Allocator& alloc);
template <class InputIt>
std::unordered_map(InputIt first, InputIt last,
                  size_type bucket_count = /* implementation-defined */,
                  const Hash& hash = Hash(),
                  const key_equal& equal = key_equal(),
                  const Allocator& alloc = Allocator());
std::unordered_map(const std::unordered_map& other);
std::unordered_map(std::unordered_map&& other);
std::unordered_map(std::initializer_list<value_type> init,
                  size_type bucket_count = /* implementation-defined */,
                  const Hash& hash = Hash(),
                  const key_equal& equal = key_equal(),
                  const Allocator& alloc = Allocator());

// 元素访问
T& operator[](const Key& key);
T& operator[](Key&& key);
T& at(const Key& key);
const T& at(const Key& key) const;

// 迭代器
iterator begin() noexcept;
const_iterator begin() const noexcept;
const_iterator cbegin() const noexcept;

iterator end() noexcept;
const_iterator end() const noexcept;
const_iterator cend() const noexcept;

// 容量
bool empty() const noexcept;
size_type size() const noexcept;
size_type max_size() const noexcept;

// 修改器
void clear() noexcept;

std::pair<iterator, bool> insert(const value_type& value);
template <class P>
std::pair<iterator, bool> insert(P&& value);
iterator insert(const_iterator hint, const value_type& value);
template <class P>
iterator insert(const_iterator hint, P&& value);
template <class InputIt>
void insert(InputIt first, InputIt last);
void insert(std::initializer_list<value_type> ilist);

template <class... Args>
std::pair<iterator, bool> emplace(Args&&... args);
template <class... Args>
iterator emplace_hint(const_iterator hint, Args&&... args);

iterator erase(const_iterator pos);
iterator erase(const_iterator first, const_iterator last);
size_type erase(const key_type& key);

void swap(std::unordered_map& other) noexcept;

// 查找
iterator find(const Key& key);
const_iterator find(const Key& key) const;

size_type count(const Key& key) const;

std::pair<iterator, iterator> equal_range(const Key& key);
std::pair<const_iterator, const_iterator> equal_range(const Key& key) const;

// 哈希策略
size_type bucket_count() const noexcept;
size_type max_bucket_count() const noexcept;
size_type bucket_size(size_type n) const;
size_type bucket(const Key& key) const;

float load_factor() const noexcept;
float max_load_factor() const noexcept;
void max_load_factor(float ml);
void rehash(size_type count);
void reserve(size_type count);
```

### `<algorithm>` - 算法

#### 非修改序列操作
```cpp
// 查找
std::find(InputIt first, InputIt last, const T& value);
std::find_if(InputIt first, InputIt last, UnaryPredicate pred);
std::find_if_not(InputIt first, InputIt last, UnaryPredicate pred);

// 相邻查找
std::adjacent_find(ForwardIt first, ForwardIt last);
std::adjacent_find(ForwardIt first, ForwardIt last, BinaryPredicate p);

// 计数
std::count(InputIt first, InputIt last, const T& value);
std::count_if(InputIt first, InputIt last, UnaryPredicate pred);

// 不匹配
std::mismatch(InputIt1 first1, InputIt1 last1, InputIt2 first2);
std::mismatch(InputIt1 first1, InputIt1 last1, InputIt2 first2, BinaryPredicate p);

// 相等
bool std::equal(InputIt1 first1, InputIt1 last1, InputIt2 first2);
bool std::equal(InputIt1 first1, InputIt1 last1, InputIt2 first2,
               BinaryPredicate p);
bool std::equal(InputIt1 first1, InputIt1 last1,
               InputIt2 first1, InputIt2 last2);

// 搜索
ForwardIt1 std::search(ForwardIt1 first1, ForwardIt1 last1,
                      ForwardIt2 first2, ForwardIt2 last2);
ForwardIt1 std::search(ForwardIt1 first1, ForwardIt1 last1,
                      ForwardIt2 first2, ForwardIt2 last2,
                      BinaryPredicate p);

// 搜索 n 个连续
ForwardIt std::search_n(ForwardIt first, ForwardIt last,
                        Size count, const T& value);
ForwardIt std::search_n(ForwardIt first, ForwardIt last,
                        Size count, const T& value,
                        BinaryPredicate p);

// 查找最后一个
ForwardIt std::find_end(ForwardIt1 first1, ForwardIt1 last1,
                       ForwardIt2 first2, ForwardIt2 last2);
ForwardIt std::find_end(ForwardIt1 first1, ForwardIt1 last1,
                       ForwardIt2 first2, ForwardIt2 last2,
                       BinaryPredicate p);
```

#### 修改序列操作
```cpp
// 拷贝
OutputIt std::copy(InputIt first, InputIt last, OutputIt d_first);
OutputIt std::copy_if(InputIt first, InputIt last,
                     OutputIt d_first, UnaryPredicate pred);
OutputIt std::copy_n(InputIt first, Size count, OutputIt result);

// 移动
OutputIt std::move(InputIt first, InputIt last, OutputIt d_first);
OutputIt std::move_backward(BidirIt1 last, BidirIt1 first,
                           BidirIt2 d_last);

// 填充
void std::fill(ForwardIt first, ForwardIt last, const T& value);
OutputIt std::fill_n(OutputIt first, Size count, const T& value);

// 变换
OutputIt std::transform(InputIt first1, InputIt last1, OutputIt d_first,
                      UnaryOperation unary_op);
OutputIt std::transform(InputIt first1, InputIt last1,
                      InputIt2 first2, OutputIt d_first,
                      BinaryOperation binary_op);

// 生成
void std::generate(ForwardIt first, ForwardIt last, Generator g);
OutputIt std::generate_n(OutputIt first, Size count, Generator g);

// 移除
ForwardIt std::remove(ForwardIt first, ForwardIt last, const T& value);
ForwardIt std::remove_if(ForwardIt first, ForwardIt last,
                         UnaryPredicate pred);

OutputIt std::remove_copy(InputIt first, InputIt last,
                          OutputIt d_first, const T& value);
OutputIt std::remove_copy_if(InputIt first, InputIt last,
                             OutputIt d_first, UnaryPredicate pred);

// 替换
void std::replace(ForwardIt first, ForwardIt last,
                 const T& old_value, const T& new_value);
void std::replace_if(ForwardIt first, ForwardIt last,
                    UnaryPredicate p, const T& new_value);
OutputIt std::replace_copy(InputIt first, InputIt last,
                          OutputIt d_first,
                          const T& old_value, const T& new_value);
OutputIt std::replace_copy_if(InputIt first, InputIt last,
                             OutputIt d_first,
                             UnaryPredicate p, const T& new_value);

// 反转
void std::reverse(BidirIt first, BidirIt last);
OutputIt std::reverse_copy(BidirIt first, BidirIt last, OutputIt d_first);

// 旋转
ForwardIt std::rotate(ForwardIt first, ForwardIt n_first, ForwardIt last);
OutputIt std::rotate_copy(ForwardIt first, ForwardIt n_first,
                         ForwardIt last, OutputIt d_first);

// 打乱
void std::random_shuffle(RandomIt first, RandomIt last);
void std::random_shuffle(RandomIt first, RandomIt last,
                       RandomFunc& r);
void std::shuffle(RandomIt first, RandomIt last,
                URNG&& g);

// 去除重复
ForwardIt std::unique(ForwardIt first, ForwardIt last);
ForwardIt std::unique(ForwardIt first, ForwardIt last, BinaryPredicate p);
OutputIt std::unique_copy(InputIt first, InputIt last, OutputIt d_first);
OutputIt std::unique_copy(InputIt first, InputIt last, OutputIt d_first,
                         BinaryPredicate p);
```

#### 分区操作
```cpp
BidirIt std::partition(BidirIt first, BidirIt last, UnaryPredicate p);
bool std::is_partitioned(InputIt first, InputIt last, UnaryPredicate p);
BidirIt std::stable_partition(BidirIt first, BidirIt last, UnaryPredicate p);

BidirIt std::partition_point(BidirIt first, BidirIt last,
                            UnaryPredicate p);
```

#### 排序操作
```cpp
// 排序
void std::sort(RandomIt first, RandomIt last);
void std::sort(RandomIt first, RandomIt last, Compare comp);

// 稳定排序
void std::stable_sort(RandomIt first, RandomIt last);
void std::stable_sort(RandomIt first, RandomIt last, Compare comp);

// 部分排序
void std::partial_sort(RandomIt first, RandomIt middle, RandomIt last);
void std::partial_sort(RandomIt first, RandomIt middle, RandomIt last,
                    Compare comp);

// 堆操作
void std::make_heap(RandomIt first, RandomIt last);
void std::make_heap(RandomIt first, RandomIt last, Compare comp);

void std::push_heap(RandomIt first, RandomIt last);
void std::push_heap(RandomIt first, RandomIt last, Compare comp);

void std::pop_heap(RandomIt first, RandomIt last);
void std::pop_heap(RandomIt first, RandomIt last, Compare comp);

void std::sort_heap(RandomIt first, RandomIt last);
void std::sort_heap(RandomIt first, RandomIt last, Compare comp);

// 第 n 元素
RandomIt std::nth_element(RandomIt first, RandomIt nth, RandomIt last);
RandomIt std::nth_element(RandomIt first, RandomIt nth, RandomIt last,
                         Compare comp);
```

#### 二分搜索
```cpp
// 检查是否包含（需要已排序）
bool std::binary_search(ForwardIt first, ForwardIt last, const T& value);
bool std::binary_search(ForwardIt first, ForwardIt last, const T& value,
                       Compare comp);

// 下界
ForwardIt std::lower_bound(ForwardIt first, ForwardIt last,
                         const T& value);
ForwardIt std::lower_bound(ForwardIt first, ForwardIt last,
                         const T& value, Compare comp);

// 上界
ForwardIt std::upper_bound(ForwardIt first, ForwardIt last,
                         const T& value);
ForwardIt std::upper_bound(ForwardIt first, ForwardIt last,
                         const T& value, Compare comp);

// 范围
std::pair<ForwardIt, ForwardIt>
std::equal_range(ForwardIt first, ForwardIt last, const T& value);
std::pair<ForwardIt, ForwardIt>
std::equal_range(ForwardIt first, ForwardIt last, const T& value,
                Compare comp);
```

#### 合并操作
```cpp
OutputIt std::merge(InputIt1 first1, InputIt1 last1,
                   InputIt2 first2, InputIt2 last2,
                   OutputIt d_first);
OutputIt std::merge(InputIt1 first1, InputIt1 last1,
                   InputIt2 first2, InputIt2 last2,
                   OutputIt d_first, Compare comp);

BidirIt std::inplace_merge(BidirIt first, BidirIt middle, BidirIt last);
BidirIt std::inplace_merge(BidirIt first, BidirIt middle, BidirIt last,
                           Compare comp);

// 包含
bool std::includes(InputIt1 first1, InputIt1 last1,
                  InputIt2 first2, InputIt2 last2);
bool std::includes(InputIt1 first1, InputIt1 last1,
                  InputIt2 first2, InputIt2 last2,
                  Compare comp);

// 集合操作
OutputIt std::set_union(InputIt1 first1, InputIt1 last1,
                        InputIt2 first2, InputIt2 last2,
                        OutputIt d_first);
OutputIt std::set_union(InputIt1 first1, InputIt1 last1,
                        InputIt2 first2, InputIt2 last2,
                        OutputIt d_first, Compare comp);

OutputIt std::set_intersection(InputIt1 first1, InputIt1 last1,
                             InputIt2 first2, InputIt2 last2,
                             OutputIt d_first);
OutputIt std::set_intersection(InputIt1 first1, InputIt1 last1,
                             InputIt2 first2, InputIt2 last2,
                             OutputIt d_first, Compare comp);

OutputIt std::set_difference(InputIt1 first1, InputIt1 last1,
                           InputIt2 first2, InputIt2 last2,
                           OutputIt d_first);
OutputIt std::set_difference(InputIt1 first1, InputIt1 last1,
                           InputIt2 first2, InputIt2 last2,
                           OutputIt d_first, Compare comp);

OutputIt std::set_symmetric_difference(InputIt1 first1, InputIt1 last1,
                                      InputIt2 first2, InputIt2 last2,
                                      OutputIt d_first);
OutputIt std::set_symmetric_difference(InputIt1 first1, InputIt1 last1,
                                      InputIt2 first2, InputIt2 last2,
                                      OutputIt d_first, Compare comp);
```

#### 堆操作
```cpp
bool std::is_heap(RandomIt first, RandomIt last);
bool std::is_heap(RandomIt first, RandomIt last, Compare comp);

RandomIt std::is_heap_until(RandomIt first, RandomIt last);
RandomIt std::is_heap_until(RandomIt first, RandomIt last, Compare comp);
```

#### 最小/最大
```cpp
const T& std::min(const T& a, const T& b);
const T& std::min(const T& a, const T& b, Compare comp);
template <class T>
T std::min(std::initializer_list<T> ilist);
template <class T, class Compare>
T std::min(std::initializer_list<T> ilist, Compare comp);

const T& std::max(const T& a, const T& b);
const T& std::max(const T& a, const T& b, Compare comp);
template <class T>
T std::max(std::initializer_list<T> ilist);
template <class T, class Compare>
T std::max(std::initializer_list<T> ilist, Compare comp);

std::pair<const T&, const T&> std::minmax(const T& a, const T& b);
std::pair<const T&, const T&> std::minmax(const T& a, const T& b,
                                        Compare comp);
template <class T>
std::pair<T, T> std::minmax(std::initializer_list<T> ilist);
template <class T, class Compare>
std::pair<T, T> std::minmax(std::initializer_list<T> ilist,
                           Compare comp);
```

#### 比较操作
```cpp
bool std::lexicographical_compare(InputIt1 first1, InputIt1 last1,
                               InputIt2 first2, InputIt2 last2);
bool std::lexicographical_compare(InputIt1 first1, InputIt1 last1,
                               InputIt2 first2, InputIt2 last2,
                               Compare comp);

// 排列
bool std::next_permutation(BidirIt first, BidirIt last);
bool std::next_permutation(BidirIt first, BidirIt last, Compare comp);

bool std::prev_permutation(BidirIt first, BidirIt last);
bool std::prev_permutation(BidirIt first, BidirIt last, Compare comp);
```

#### 数值操作
```cpp
// 累加
T std::accumulate(InputIt first, InputIt last, T init);
T std::accumulate(InputIt first, InputIt last, T init, BinaryOperation op);

// 内积
T std::inner_product(InputIt1 first1, InputIt1 last1,
                   InputIt2 first2, T init);
T std::inner_product(InputIt1 first1, InputIt1 last1,
                   InputIt2 first2, T init,
                   BinaryOperation1 op1, BinaryOperation2 op2);

// 相邻差分
OutputIt std::adjacent_difference(InputIt first, InputIt last,
                                 OutputIt d_first);
OutputIt std::adjacent_difference(InputIt first, InputIt last,
                                 OutputIt d_first,
                                 BinaryOperation op);

// 部分和
OutputIt std::partial_sum(InputIt first, InputIt last,
                        OutputIt d_first);
OutputIt std::partial_sum(InputIt first, InputIt last,
                        OutputIt d_first, BinaryOperation op);

// iota (C++11)
void std::iota(ForwardIt first, ForwardIt last, T value);
```

#### C 操作
```cpp
void std::copy(InputIt first, InputIt last, OutputIt d_first);
void std::move(InputIt first, InputIt last, OutputIt d_first);
void std::swap(T& a, T& b);
```

### `<memory>` - 内存管理

#### 智能指针
```cpp
// unique_ptr (C++11)
template <class T, class Deleter = std::default_delete<T>>
class std::unique_ptr;

template <class T, class... Args>
std::unique_ptr<T> std::make_unique(Args&&... args);  // C++14

// shared_ptr (C++11)
template <class T>
class std::shared_ptr;

template <class T, class... Args>
std::shared_ptr<T> std::make_shared(Args&&... args);

// weak_ptr (C++11)
template <class T>
class std::weak_ptr;

// auto_ptr (C++98, C++11 弃用, C++17 移除)
// 不推荐使用
```

#### 辅助函数
```cpp
template <class T, class U>
std::shared_ptr<T> std::static_pointer_cast(const std::shared_ptr<U>& r);

template <class T, class U>
std::shared_ptr<T> std::dynamic_pointer_cast(const std::shared_ptr<U>& r);

template <class T, class U>
std::shared_ptr<T> std::const_pointer_cast(const std::shared_ptr<U>& r);

template <class T, class U>
std::shared_ptr<T> std::reinterpret_pointer_cast(const std::shared_ptr<U>& r);

template <class Deleter, class T>
Deleter* std::get_deleter(const std::shared_ptr<T>& p);

template <class T, class U>
bool std::owner_before(const std::shared_ptr<T>& lhs,
                      const std::shared_ptr<U>& rhs);

template <class T, class U>
bool std::owner_before(const std::shared_ptr<T>& lhs,
                      const std::weak_ptr<U>& rhs);

template <class T, class U>
bool std::owner_before(const std::weak_ptr<T>& lhs,
                      const std::shared_ptr<U>& rhs);

template <class T, class U>
bool std::owner_before(const std::weak_ptr<T>& lhs,
                      const std::weak_ptr<U>& rhs);
```

#### 分配器
```cpp
template <class T>
class std::allocator;

template <class T, class Alloc = std::allocator<T>>
class std::allocator_traits;
```

### `<thread>` - 线程 (C++11)

```cpp
// 线程
class std::thread;

std::thread::thread() noexcept;
template <class Function, class... Args>
explicit std::thread::thread(Function&& f, Args&&... args);
~thread();

std::thread::thread(const std::thread&) = delete;
std::thread& operator=(const std::thread&) = delete;
std::thread::thread(std::thread&& other) noexcept;
std::thread& operator=(std::thread&& other) noexcept;

bool joinable() const noexcept;
void join();
void detach();
id get_id() const noexcept;
native_handle_type native_handle();

static unsigned int hardware_concurrency() noexcept;

// this_thread 命名空间
namespace std::this_thread {
    thread::id get_id() noexcept;
    void yield() noexcept;
    template <class Clock, class Duration>
    void sleep_until(const std::chrono::time_point<Clock, Duration>& abs_time);
    template <class Rep, class Period>
    void sleep_for(const std::chrono::duration<Rep, Period>& rel_time);
}

// jthread (C++20)
class std::jthread;
```

### `<mutex>` - 互斥锁 (C++11)

```cpp
// mutex
class std::mutex;
void lock();
bool try_lock();
void unlock();

// recursive_mutex
class std::recursive_mutex;
void lock();
bool try_lock();
void unlock();

// timed_mutex (C++11)
class std::timed_mutex;
void lock();
bool try_lock();
template <class Rep, class Period>
bool try_lock_for(const std::chrono::duration<Rep, Period>& rel_time);
template <class Clock, class Duration>
bool try_lock_until(const std::chrono::time_point<Clock, Duration>& abs_time);
void unlock();

// recursive_timed_mutex (C++11)
class std::recursive_timed_mutex;

// lock_guard (C++11)
template <class Mutex>
class std::lock_guard;
explicit lock_guard(Mutex& m);
lock_guard(Mutex& m, std::adopt_lock_t);
~lock_guard();

// unique_lock (C++11)
template <class Mutex>
class std::unique_lock;
explicit unique_lock(Mutex& m);
unique_lock(Mutex& m, std::defer_lock_t);
unique_lock(Mutex& m, std::try_to_lock_t);
unique_lock(Mutex& m, std::adopt_lock_t);
template <class Clock, class Duration>
unique_lock(Mutex& m, const std::chrono::time_point<Clock, Duration>& abs_time);
template <class Rep, class Period>
unique_lock(Mutex& m, const std::chrono::duration<Rep, Period>& rel_time);
~unique_lock();

void lock();
bool try_lock();
template <class Rep, class Period>
bool try_lock_for(const std::chrono::duration<Rep, Period>& rel_time);
template <class Clock, class Duration>
bool try_lock_until(const std::chrono::time_point<Clock, Duration>& abs_time);
void unlock();
Mutex* mutex() const noexcept;
bool owns_lock() const noexcept;
explicit operator bool() const noexcept;
Mutex* release();

// scoped_lock (C++17)
template <class... MutexTypes>
class std::scoped_lock;
explicit scoped_lock(MutexTypes&... m);
explicit scoped_lock(std::adopt_lock_t, MutexTypes&... m);
~scoped_lock();

// shared_mutex (C++17)
class std::shared_mutex;
void lock();
bool try_lock();
void unlock();
void lock_shared();
bool try_lock_shared();
void unlock_shared();

// shared_lock (C++14)
template <class Mutex>
class std::shared_lock;
explicit shared_lock(Mutex& m);
shared_lock(Mutex& m, std::defer_lock_t);
shared_lock(Mutex& m, std::try_to_lock_t);
template <class Clock, class Duration>
shared_lock(Mutex& m, const std::chrono::time_point<Clock, Duration>& abs_time);
template <class Rep, class Period>
shared_lock(Mutex& m, const std::chrono::duration<Rep, Period>& rel_time);
~shared_lock();

void lock();
bool try_lock();
template <class Rep, class Period>
bool try_lock_for(const std::chrono::duration<Rep, Period>& rel_time);
template <class Clock, class Duration>
bool try_lock_until(const std::chrono::time_point<Clock, Duration>& abs_time);
void unlock();
void lock_shared();
bool try_lock_shared();
template <class Rep, class Period>
bool try_lock_shared_for(const std::chrono::duration<Rep, Period>& rel_time);
template <class Clock, class Duration>
bool try_lock_shared_until(const std::chrono::time_point<Clock, Duration>& abs_time);
void unlock_shared();
Mutex* mutex() const noexcept;
bool owns_lock() const noexcept;
explicit operator bool() const noexcept;
Mutex* release();

// 通用锁
template <class Lock1, class Lock2, class... Locks>
int std::try_lock(Lock1& lock1, Lock2& lock2, Locks&... locks);

template <class Lock1, class Lock2, class... Locks>
void std::lock(Lock1& lock1, Lock2& lock2, Locks&... locks);
```

### `<condition_variable>` - 条件变量 (C++11)

```cpp
// condition_variable
class std::condition_variable;

condition_variable();
~condition_variable();

void notify_one() noexcept;
void notify_all() noexcept;

void wait(std::unique_lock<std::mutex>& lock);
template <class Predicate>
void wait(std::unique_lock<std::mutex>& lock, Predicate pred);

template <class Clock, class Duration>
std::cv_status wait_until(std::unique_lock<std::mutex>& lock,
                         const std::chrono::time_point<Clock, Duration>& abs_time);

template <class Clock, class Duration, class Predicate>
bool wait_until(std::unique_lock<std::mutex>& lock,
               const std::chrono::time_point<Clock, Duration>& abs_time,
               Predicate pred);

template <class Rep, class Period>
std::cv_status wait_for(std::unique_lock<std::mutex>& lock,
                        const std::chrono::duration<Rep, Period>& rel_time);

template <class Rep, class Period, class Predicate>
bool wait_for(std::unique_lock<std::mutex>& lock,
             const std::chrono::duration<Rep, Period>& rel_time,
             Predicate pred);

// condition_variable_any (C++11)
template <class Lock>
class std::condition_variable_any;
```

### `<atomic>` - 原子操作 (C++11)

```cpp
// 原子类型
template <class T>
class std::atomic;
template <class T>
class std::atomic_ref;

// 特化类型
std::atomic_bool;    // std::atomic<bool>
std::atomic_char;
std::atomic_schar;
std::atomic_uchar;
std::atomic_short;
std::atomic_ushort;
std::atomic_int;
std::atomic_uint;
std::atomic_long;
std::atomic_ulong;
std::atomic_llong;
std::atomic_ullong;
std::atomic_char16_t;
std::atomic_char32_t;
std::atomic_wchar_t;
std::atomic_int8_t;
std::atomic_uint8_t;
std::atomic_int16_t;
std::atomic_uint16_t;
std::atomic_int32_t;
std::atomic_uint32_t;
std::atomic_int64_t;
std::atomic_uint64_t;
std::atomic_intptr_t;
std::atomic_uintptr_t;
std::atomic_size_t;
std::atomic_ptrdiff_t;
std::atomic_intmax_t;
std::atomic_uintmax_t;

// 成员函数
void store(T desired, std::memory_order order = std::memory_order_seq_cst) volatile noexcept;
void store(T desired, std::memory_order order = std::memory_order_seq_cst) noexcept;
T load(std::memory_order order = std::memory_order_seq_cst) const volatile noexcept;
T load(std::memory_order order = std::memory_order_seq_cst) const noexcept;
operator T() const volatile noexcept;
operator T() const noexcept;
T exchange(T desired, std::memory_order order = std::memory_order_seq_cst) volatile noexcept;
T exchange(T desired, std::memory_order order = std::memory_order_seq_cst) noexcept;
bool compare_exchange_weak(T& expected, T desired,
                         std::memory_order success,
                         std::memory_order failure) volatile noexcept;
bool compare_exchange_weak(T& expected, T desired,
                         std::memory_order success,
                         std::memory_order failure) noexcept;
bool compare_exchange_strong(T& expected, T desired,
                           std::memory_order success,
                           std::memory_order failure) volatile noexcept;
bool compare_exchange_strong(T& expected, T desired,
                           std::memory_order success,
                           std::memory_order failure) noexcept;
T fetch_add(T arg, std::memory_order order = std::memory_order_seq_cst) volatile noexcept;
T fetch_add(T arg, std::memory_order order = std::memory_order_seq_cst) noexcept;
T fetch_sub(T arg, std::memory_order order = std::memory_order_seq_cst) volatile noexcept;
T fetch_sub(T arg, std::memory_order order = std::memory_order_seq_cst) noexcept;
T fetch_and(T arg, std::memory_order order = std::memory_order_seq_cst) volatile noexcept;
T fetch_and(T arg, std::memory_order order = std::memory_order_seq_cst) noexcept;
T fetch_or(T arg, std::memory_order order = std::memory_order_seq_cst) volatile noexcept;
T fetch_or(T arg, std::memory_order order = std::memory_order_seq_cst) noexcept;
T fetch_xor(T arg, std::memory_order order = std::memory_order_seq_cst) volatile noexcept;
T fetch_xor(T arg, std::memory_order order = std::memory_order_seq_cst) noexcept;
T operator++() volatile noexcept;
T operator++() noexcept;
T operator++(int) volatile noexcept;
T operator++(int) noexcept;
T operator--() volatile noexcept;
T operator--() noexcept;
T operator--(int) volatile noexcept;
T operator--(int) noexcept;
T operator+=(T arg) volatile noexcept;
T operator+=(T arg) noexcept;
T operator-=(T arg) volatile noexcept;
T operator-=(T arg) noexcept;
T operator&=(T arg) volatile noexcept;
T operator&=(T arg) noexcept;
T operator|=(T arg) volatile noexcept;
T operator|=(T arg) noexcept;
T operator^=(T arg) volatile noexcept;
T operator^=(T arg) noexcept;

// 辅助函数
template <class T>
bool std::atomic_is_lock_free(const volatile std::atomic<T>* obj) noexcept;
template <class T>
bool std::atomic_is_lock_free(const std::atomic<T>* obj) noexcept;
template <class T>
void std::atomic_init(volatile std::atomic<T>* obj, T desired) noexcept;
template <class T>
void std::atomic_init(std::atomic<T>* obj, T desired) noexcept;
template <class T>
void std::atomic_store(volatile std::atomic<T>* obj, T desired) noexcept;
template <class T>
void std::atomic_store(std::atomic<T>* obj, T desired) noexcept;
template <class T>
void std::atomic_store_explicit(volatile std::atomic<T>* obj, T desired,
                               std::memory_order order) noexcept;
template <class T>
void std::atomic_store_explicit(std::atomic<T>* obj, T desired,
                               std::memory_order order) noexcept;
template <class T>
T std::atomic_load(const volatile std::atomic<T>* obj) noexcept;
template <class T>
T std::atomic_load(const std::atomic<T>* obj) noexcept;
template <class T>
T std::atomic_load_explicit(const volatile std::atomic<T>* obj,
                           std::memory_order order) noexcept;
template <class T>
T std::atomic_load_explicit(const std::atomic<T>* obj,
                           std::memory_order order) noexcept;
template <class T>
T std::atomic_exchange(volatile std::atomic<T>* obj, T desired) noexcept;
template <class T>
T std::atomic_exchange(std::atomic<T>* obj, T desired) noexcept;
template <class T>
T std::atomic_exchange_explicit(volatile std::atomic<T>* obj, T desired,
                               std::memory_order order) noexcept;
template <class T>
T std::atomic_exchange_explicit(std::atomic<T>* obj, T desired,
                               std::memory_order order) noexcept;
template <class T>
bool std::atomic_compare_exchange_weak(volatile std::atomic<T>* obj,
                                     T* expected, T desired) noexcept;
template <class T>
bool std::atomic_compare_exchange_weak(std::atomic<T>* obj,
                                     T* expected, T desired) noexcept;
template <class T>
bool std::atomic_compare_exchange_strong(volatile std::atomic<T>* obj,
                                       T* expected, T desired) noexcept;
template <class T>
bool std::atomic_compare_exchange_strong(std::atomic<T>* obj,
                                       T* expected, T desired) noexcept;
template <class T>
bool std::atomic_compare_exchange_weak_explicit(volatile std::atomic<T>* obj,
                                             T* expected, T desired,
                                             std::memory_order success,
                                             std::memory_order failure) noexcept;
template <class T>
bool std::atomic_compare_exchange_weak_explicit(std::atomic<T>* obj,
                                             T* expected, T desired,
                                             std::memory_order success,
                                             std::memory_order failure) noexcept;
template <class T>
bool std::atomic_compare_exchange_strong_explicit(volatile std::atomic<T>* obj,
                                               T* expected, T desired,
                                               std::memory_order success,
                                               std::memory_order failure) noexcept;
template <class T>
bool std::atomic_compare_exchange_strong_explicit(std::atomic<T>* obj,
                                               T* expected, T desired,
                                               std::memory_order success,
                                               std::memory_order failure) noexcept;
template <class T>
T std::atomic_fetch_add(volatile std::atomic<T>* obj, T arg) noexcept;
template <class T>
T std::atomic_fetch_add(std::atomic<T>* obj, T arg) noexcept;
// ... fetch_sub, fetch_and, fetch_or, fetch_xor

// 标志
struct std::atomic_flag;
bool test_and_set(std::memory_order order = std::memory_order_seq_cst) volatile noexcept;
bool test_and_set(std::memory_order order = std::memory_order_seq_cst) noexcept;
void clear(std::memory_order order = std::memory_order_seq_cst) volatile noexcept;
void clear(std::memory_order order = std::memory_order_seq_cst) noexcept;

#define ATOMIC_FLAG_INIT
bool atomic_flag_test_and_set(volatile std::atomic_flag* obj) noexcept;
bool atomic_flag_test_and_set(std::atomic_flag* obj) noexcept;
void atomic_flag_clear(volatile std::atomic_flag* obj) noexcept;
void atomic_flag_clear(std::atomic_flag* obj) noexcept;
void atomic_flag_test_and_set_explicit(volatile std::atomic_flag* obj,
                                     std::memory_order order) noexcept;
void atomic_flag_test_and_set_explicit(std::atomic_flag* obj,
                                     std::memory_order order) noexcept;
void atomic_flag_clear_explicit(volatile std::atomic_flag* obj,
                               std::memory_order order) noexcept;
void atomic_flag_clear_explicit(std::atomic_flag* obj,
                               std::memory_order order) noexcept;

// 内存顺序
enum class std::memory_order : int {
    relaxed, consume, acquire, release, acq_rel, seq_cst
};

// Fence
void std::atomic_thread_fence(std::memory_order order) noexcept;
void std::atomic_signal_fence(std::memory_order order) noexcept;
```

### `<chrono>` - 时间库 (C++11)

```cpp
// 时钟
class std::chrono::system_clock;
static time_point now() noexcept;
static std::time_t to_time_t(const time_point& t) noexcept;
static time_point from_time_t(std::time_t t) noexcept;

class std::chrono::steady_clock;
static time_point now() noexcept;

class std::chrono::high_resolution_clock;
static time_point now() noexcept;

// 时间点
template <class Clock, class Duration = typename Clock::duration>
class std::chrono::time_point;

// 时间段
template <class Rep, class Period = std::ratio<1>>
class std::chrono::duration;

// 类型别名
std::chrono::nanoseconds;
std::chrono::microseconds;
std::chrono::milliseconds;
std::chrono::seconds;
std::chrono::minutes;
std::chrono::hours;

// 辅助函数
template <class Rep>
constexpr std::chrono::duration<Rep> std::chrono::abs(std::chrono::duration<Rep> d);
```

### `<regex>` - 正则表达式 (C++11)

```cpp
// basic_regex
template <class BidirIt, class Allocator = std::allocator<sub_match<BidirIt>>>
class std::basic_regex;

template <class BidirIt>
std::basic_regex(BidirIt first, BidirIt last,
                 flag_type flags = std::regex_constants::ECMAScript);
template <class ST, class SA>
std::basic_regex(const ST& s,
                 flag_type flags = std::regex_constants::ECMAScript);

// regex
typedef basic_regex<char> std::regex;
typedef basic_regex<wchar_t> std::wregex;

// sub_match
template <class BidirIt>
class std::sub_match;

// match_results
template <class BidirIt, class Allocator = std::allocator<sub_match<BidirIt>>>
class std::match_results;

// cmatch, wcmatch, smatch, wsmatch
typedef match_results<const char*> std::cmatch;
typedef match_results<const wchar_t*> std::wcmatch;
typedef match_results<std::string::const_iterator> std::smatch;
typedef match_results<std::wstring::const_iterator> std::wsmatch;

// regex_match
template <class BidirIt, class Allocator, class charT, class traits>
bool std::regex_match(BidirIt first, BidirIt last,
                     std::match_results<BidirIt, Allocator>& m,
                     const std::basic_regex<charT, traits>& e,
                     std::regex_constants::match_flag_type flags =
                     std::regex_constants::match_default);
template <class charT, class Allocator, class traits>
bool std::regex_match(const charT* str,
                     std::match_results<const charT*, Allocator>& m,
                     const std::basic_regex<charT, traits>& e,
                     std::regex_constants::match_flag_type flags =
                     std::regex_constants::match_default);
template <class ST, class SA, class charT, class traits>
bool std::regex_match(const ST& s,
                     std::match_results<typename ST::const_iterator, SA>& m,
                     const std::basic_regex<charT, traits>& e,
                     std::regex_constants::match_flag_type flags =
                     std::regex_constants::match_default);
template <class charT, class traits>
bool std::regex_match(const charT* str,
                     const std::basic_regex<charT, traits>& e,
                     std::regex_constants::match_flag_type flags =
                     std::regex_constants::match_default);
template <class ST, class charT, class traits>
bool std::regex_match(const ST& s,
                     const std::basic_regex<charT, traits>& e,
                     std::regex_constants::match_flag_type flags =
                     std::regex_constants::match_default);

// regex_search
template <class BidirIt, class Allocator, class charT, class traits>
bool std::regex_search(BidirIt first, BidirIt last,
                      std::match_results<BidirIt, Allocator>& m,
                      const std::basic_regex<charT, traits>& e,
                      std::regex_constants::match_flag_type flags =
                      std::regex_constants::match_default);
template <class charT, class Allocator, class traits>
bool std::regex_search(const charT* str,
                      std::match_results<const charT*, Allocator>& m,
                      const std::basic_regex<charT, traits>& e,
                      std::regex_constants::match_flag_type flags =
                      std::regex_constants::match_default);
template <class ST, class SA, class charT, class traits>
bool std::regex_search(const ST& s,
                      std::match_results<typename ST::const_iterator, SA>& m,
                      const std::basic_regex<charT, traits>& e,
                      std::regex_constants::match_flag_type flags =
                      std::regex_constants::match_default);
template <class BidirIt, class charT, class traits>
bool std::regex_search(BidirIt first, BidirIt last,
                      const std::basic_regex<charT, traits>& e,
                      std::regex_constants::match_flag_type flags =
                      std::regex_constants::match_default);
template <class charT, class traits>
bool std::regex_search(const charT* str,
                      const std::basic_regex<charT, traits>& e,
                      std::regex_constants::match_flag_type flags =
                      std::regex_constants::match_default);
template <class ST, class charT, class traits>
bool std::regex_search(const ST& s,
                      const std::basic_regex<charT, traits>& e,
                      std::regex_constants::match_flag_type flags =
                      std::regex_constants::match_default);

// regex_replace
template <class OutputIt, class BidirIt, class traits,
          class charT, class ST, class SA>
OutputIt std::regex_replace(OutputIt out, BidirIt first, BidirIt last,
                            const std::basic_regex<charT, traits>& e,
                            const ST& fmt,
                            std::regex_constants::match_flag_type flags =
                            std::regex_constants::match_default);
template <class traits, class charT, class ST, class SA>
std::basic_string<charT> std::regex_replace(
    const std::basic_string<charT, ST, SA>& s,
    const std::basic_regex<charT, traits>& e, const ST& fmt,
    std::regex_constants::match_flag_type flags =
    std::regex_constants::match_default);
template <class traits, class charT, class ST, class SA>
std::basic_string<charT> std::regex_replace(
    const charT* s,
    const std::basic_regex<charT, traits>& e, const ST& fmt,
    std::regex_constants::match_flag_type flags =
    std::regex_constants::match_default);
```

### `<functional>` - 函数对象

```cpp
// 函数包装器
template <class Signature>
class std::function;

// 引用包装器
template <class T>
class std::reference_wrapper;

template <class T>
std::reference_wrapper<T> std::ref(T& t) noexcept;
template <class T>
std::reference_wrapper<const T> std::cref(const T& t) noexcept;

// 算术运算
std::plus<T>;
std::minus<T>;
std::multiplies<T>;
std::divides<T>;
std::modulus<T>;
std::negate<T>;

// 比较运算
std::equal_to<T>;
std::not_equal_to<T>;
std::greater<T>;
std::less<T>;
std::greater_equal<T>;
std::less_equal<T>;

// 逻辑运算
std::logical_and<T>;
std::logical_or<T>;
std::logical_not<T>;

// 位运算
std::bit_and<T>;
std::bit_or<T>;
std::bit_xor<T>;
std::bit_not<T>;

// 隐藏 identity (C++20)
std::identity;

// 移动 (C++14)
template <class T>
constexpr std::remove_reference_t<T>&& std::move(T&& t) noexcept;

// 转发 (C++11)
template <class T>
constexpr T&& std::forward(typename std::remove_reference<T>::type& t) noexcept;
template <class T>
constexpr T&& std::forward(typename std::remove_reference<T>::type&& t) noexcept;

// 交换 (C++11)
template <class T>
constexpr void std::swap(T& a, T& b) noexcept(std::is_nothrow_move_constructible<T>::value &&
                                          std::is_nothrow_move_assignable<T>::value);

// 位运算交换 (C++20)
constexpr std::byte std::rotl(std::byte b, unsigned int s) noexcept;
constexpr std::byte std::rotr(std::byte b, unsigned int s) noexcept;
constexpr unsigned int std::countl_zero(unsigned int x) noexcept;
constexpr unsigned int std::countl_one(unsigned int x) noexcept;
constexpr unsigned int std::countr_zero(unsigned int x) noexcept;
constexpr unsigned int std::countr_one(unsigned int x) noexcept;
constexpr unsigned int std::popcount(unsigned int x) noexcept;

// 哈希 (C++11)
template <class Key>
struct std::hash;
```

### `<random>` - 随机数生成 (C++11)

```cpp
// 线性同余引擎
template <class UIntType, UIntType a, UIntType c, UIntType m>
class std::minstd_rand0;
template <class UIntType, UIntType a, UIntType c, UIntType m>
class std::minstd_rand;

// 梅森旋转算法
class std::mt19937;
class std::mt19937_64;

// 随机设备
class std::random_device;

// 均匀分布
template <class IntType = int>
class std::uniform_int_distribution;

template <class RealType = double>
class std::uniform_real_distribution;

// 伯努利分布
class std::bernoulli_distribution;
template <class IntType = int>
class std::binomial_distribution;

// 几何分布
template <class IntType = int>
class std::geometric_distribution;

// 负二项分布
template <class IntType = int>
class std::negative_binomial_distribution;

// 泊松分布
template <class IntType = int>
class std::poisson_distribution;

// 指数分布
template <class RealType = double>
class std::exponential_distribution;

// 正态分布
template <class RealType = double>
class std::normal_distribution;
template <class RealType = double>
class std::lognormal_distribution;

// 伽马分布
template <class RealType = double>
class std::gamma_distribution;

// 韦伯分布
template <class RealType = double>
class std::weibull_distribution;

// 极值分布
template <class RealType = double>
class std::extreme_value_distribution;

// 卡方分布
template <class RealType = double>
class std::chi_squared_distribution;

// 卡分布
template <class RealType = double>
class std::cauchy_distribution;

// 费歇尔F分布
template <class RealType = double>
class std::fisher_f_distribution;

// 学生t分布
template <class RealType = double>
class std::student_t_distribution;

// 离散分布
template <class IntType = int>
class std::discrete_distribution;
template <class RealType = double>
class std::piecewise_constant_distribution;
template <class RealType = double>
class std::piecewise_linear_distribution;
```

## 常用第三方库

### Boost 库

#### Boost.Asio - 网络编程
```cpp
#include <boost/asio.hpp>
```

#### Boost.Thread - 线程
```cpp
#include <boost/thread.hpp>
```

#### Boost.Filesystem - 文件系统
```cpp
#include <boost/filesystem.hpp>
```

#### Boost.SmartPtr - 智能指针
```cpp
#include <boost/smart_ptr.hpp>
#include <boost/shared_ptr.hpp>
#include <boost/scoped_ptr.hpp>
#include <boost/weak_ptr.hpp>
#include <boost/intrusive_ptr.hpp>
```

### OpenSSL - 加密库

```c
#include <openssl/ssl.h>
#include <openssl/err.h>
#include <openssl/rand.h>
```

### libcurl - HTTP 客户端

```c
#include <curl/curl.h>
```

### SQLite - 嵌入式数据库

```c
#include <sqlite3.h>
```

### SDL2 - 多媒体库

```c
#include <SDL2/SDL.h>
```

### Qt - GUI 框架

```cpp
#include <QApplication>
#include <QWidget>
#include <QPushButton>
```

### OpenCV - 计算机视觉

```cpp
#include <opencv2/opencv.hpp>
```

## 函数使用示例

### C 语言示例

#### 字符串操作
```c
#include <stdio.h>
#include <string.h>

int main() {
    char str1[50] = "Hello";
    char str2[] = "World";
    
    // 字符串连接
    strcat(str1, " ");
    strcat(str1, str2);
    printf("Concatenated: %s\n", str1);
    
    // 字符串比较
    if (strcmp(str1, "Hello World") == 0) {
        printf("Strings are equal\n");
    }
    
    // 字符串查找
    char* ptr = strstr(str1, "World");
    if (ptr != NULL) {
        printf("Found 'World' at position: %ld\n", ptr - str1);
    }
    
    // 字符串复制
    char dest[50];
    strcpy(dest, str1);
    printf("Copied: %s\n", dest);
    
    return 0;
}
```

#### 内存分配
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    // 动态分配数组
    int* arr = (int*)malloc(5 * sizeof(int));
    if (arr == NULL) {
        printf("Memory allocation failed\n");
        return 1;
    }
    
    // 初始化
    for (int i = 0; i < 5; i++) {
        arr[i] = (i + 1) * 10;
    }
    
    // 打印
    printf("Array: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
    
    // 扩容
    arr = (int*)realloc(arr, 10 * sizeof(int));
    for (int i = 5; i < 10; i++) {
        arr[i] = (i + 1) * 10;
    }
    
    // 释放
    free(arr);
    
    return 0;
}
```

### C++ 示例

#### 智能指针
```cpp
#include <iostream>
#include <memory>

struct Resource {
    Resource() { std::cout << "Resource created\n"; }
    ~Resource() { std::cout << "Resource destroyed\n"; }
    void doSomething() { std::cout << "Working...\n"; }
};

int main() {
    // unique_ptr
    auto ptr1 = std::make_unique<Resource>();
    ptr1->doSomething();
    
    // shared_ptr
    auto ptr2 = std::make_shared<Resource>();
    auto ptr3 = ptr2;
    std::cout << "Use count: " << ptr2.use_count() << std::endl;
    
    return 0;
}
```

#### 容器操作
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

int main() {
    std::vector<int> vec = {5, 2, 8, 1, 9, 3};
    
    // 排序
    std::sort(vec.begin(), vec.end());
    
    // 查找
    auto it = std::find(vec.begin(), vec.end(), 5);
    if (it != vec.end()) {
        std::cout << "Found 5 at position: " << (it - vec.begin()) << std::endl;
    }
    
    // 变换
    std::transform(vec.begin(), vec.end(), vec.begin(),
                   [](int x) { return x * 2; });
    
    // 打印
    for (int num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

#### 线程
```cpp
#include <iostream>
#include <thread>
#include <mutex>

std::mutex mtx;
int counter = 0;

void worker(int id) {
    for (int i = 0; i < 1000; i++) {
        std::lock_guard<std::mutex> lock(mtx);
        counter++;
    }
}

int main() {
    std::thread t1(worker, 1);
    std::thread t2(worker, 2);
    
    t1.join();
    t2.join();
    
    std::cout << "Counter: " << counter << std::endl;
    return 0;
}
```

这个函数库索引提供了 C/C++ 标准库中主要函数的完整列表，包括函数签名、参数说明和使用示例，可以作为快速查询的参考资料。
