# C/C++ 语法示例大全

## 目录
- [C 语言语法示例](#c-语言语法示例)
- [C++ 语法示例](#c-语法示例)
- [常见模式与最佳实践](#常见模式与最佳实践)
- [性能优化示例](#性能优化示例)
- [错误处理示例](#错误处理示例)

## C 语言语法示例

### 基础语法

#### 变量与数据类型
```c
#include <stdio.h>

int main() {
    // 基本数据类型
    int integer = 42;
    float floating = 3.14f;
    double double_precise = 3.14159265;
    char character = 'A';
    
    // 限定符
    const int CONSTANT = 100;
    volatile int signal = 0;
    static int counter = 0;
    
    // C99 指定初始化器
    struct Point { int x, y; };
    struct Point p = { .y = 10, .x = 20 };
    
    printf("Point: (%d, %d)\n", p.x, p.y);
    
    return 0;
}
```

#### 运算符
```c
#include <stdio.h>

int main() {
    int a = 10, b = 3;
    
    // 算术运算符
    printf("a + b = %d\n", a + b);   // 13
    printf("a - b = %d\n", a - b);   // 7
    printf("a * b = %d\n", a * b);   // 30
    printf("a / b = %d\n", a / b);   // 3
    printf("a %% b = %d\n", a % b);  // 1
    
    // 关系运算符
    printf("a > b: %d\n", a > b);    // 1 (true)
    printf("a == b: %d\n", a == b);  // 0 (false)
    
    // 逻辑运算符
    printf("a > 0 && b > 0: %d\n", a > 0 && b > 0);  // 1
    printf("a > 10 || b > 10: %d\n", a > 10 || b > 10); // 0
    
    // 位运算符
    printf("a & b = %d\n", a & b);    // 2
    printf("a | b = %d\n", a | b);    // 11
    printf("a ^ b = %d\n", a ^ b);    // 9
    printf("~a = %d\n", ~a);          // -11
    
    // 自增自减
    int x = 5;
    printf("x++ = %d, x = %d\n", x++, x);  // 5, 6
    printf("++x = %d, x = %d\n", ++x, x);  // 7, 7
    
    return 0;
}
```

#### 控制流
```c
#include <stdio.h>

int main() {
    // if-else
    int score = 85;
    if (score >= 90) {
        printf("A\n");
    } else if (score >= 80) {
        printf("B\n");  // 输出 B
    } else if (score >= 70) {
        printf("C\n");
    } else {
        printf("F\n");
    }
    
    // switch
    int day = 3;
    switch (day) {
        case 1: printf("Monday\n"); break;
        case 2: printf("Tuesday\n"); break;
        case 3: printf("Wednesday\n"); break;  // 输出
        default: printf("Other day\n");
    }
    
    // for 循环
    printf("For loop: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", i);
    }
    printf("\n");
    
    // while 循环
    printf("While loop: ");
    int j = 0;
    while (j < 5) {
        printf("%d ", j++);
    }
    printf("\n");
    
    // do-while 循环
    printf("Do-while loop: ");
    int k = 0;
    do {
        printf("%d ", k++);
    } while (k < 5);
    printf("\n");
    
    // break 和 continue
    printf("Break/Continue: ");
    for (int i = 0; i < 10; i++) {
        if (i == 3) continue;   // 跳过 3
        if (i == 7) break;      // 在 7 处停止
        printf("%d ", i);
    }
    printf("\n");
    
    // goto (慎用)
    printf("Goto: ");
    int n = 0;
start:
    printf("%d ", n++);
    if (n < 5) goto start;
    printf("\n");
    
    return 0;
}
```

### 函数与指针

#### 函数定义与调用
```c
#include <stdio.h>

// 函数声明
int add(int a, int b);
void swap(int* a, int* b);
int max(int a, int b, int c);

// 带默认参数的函数（C99 通过宏实现）
#define SQUARE(x) ((x) * (x))

// 可变参数函数
#include <stdarg.h>

int sum(int count, ...) {
    va_list args;
    va_start(args, count);
    
    int total = 0;
    for (int i = 0; i < count; i++) {
        total += va_arg(args, int);
    }
    
    va_end(args);
    return total;
}

// 函数指针
int (*operation)(int, int);

int add_func(int a, int b) { return a + b; }
int multiply_func(int a, int b) { return a * b; }

// 回调函数
void apply(int (*func)(int, int), int a, int b) {
    printf("Result: %d\n", func(a, b));
}

// 递归函数
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

// 斐波那契数列
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

int main() {
    // 基本函数调用
    printf("add(3, 4) = %d\n", add(3, 4));
    
    // 指针交换
    int x = 10, y = 20;
    printf("Before: x=%d, y=%d\n", x, y);
    swap(&x, &y);
    printf("After: x=%d, y=%d\n", x, y);
    
    // 最大值
    printf("max(5, 8, 3) = %d\n", max(5, 8, 3));
    
    // 宏
    printf("SQUARE(5) = %d\n", SQUARE(5));
    
    // 可变参数
    printf("sum(4, 1, 2, 3, 4) = %d\n", sum(4, 1, 2, 3, 4));
    
    // 函数指针
    operation = add_func;
    printf("add via pointer: %d\n", operation(5, 3));
    
    operation = multiply_func;
    printf("multiply via pointer: %d\n", operation(5, 3));
    
    // 回调函数
    apply(add_func, 10, 20);
    apply(multiply_func, 10, 20);
    
    // 递归
    printf("factorial(5) = %d\n", factorial(5));
    printf("fibonacci(10) = %d\n", fibonacci(10));
    
    return 0;
}

// 函数定义
int add(int a, int b) {
    return a + b;
}

void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int max(int a, int b, int c) {
    int m = a;
    if (b > m) m = b;
    if (c > m) m = c;
    return m;
}
```

#### 指针基础
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    // 基本指针
    int x = 42;
    int* ptr = &x;
    
    printf("x = %d\n", x);          // 42
    printf("&x = %p\n", (void*)&x); // 地址
    printf("*ptr = %d\n", *ptr);    // 42
    printf("ptr = %p\n", (void*)ptr);// 地址
    
    // 指针运算
    int arr[] = {10, 20, 30, 40, 50};
    int* p = arr;
    
    printf("\nPointer arithmetic:\n");
    printf("*p = %d\n", *p);      // 10
    printf("*(p+1) = %d\n", *(p+1)); // 20
    printf("p[2] = %d\n", p[2]);    // 30
    
    // 指针遍历数组
    printf("\nTraverse array with pointer:\n");
    for (int* ip = arr; ip < arr + 5; ip++) {
        printf("%d ", *ip);
    }
    printf("\n");
    
    // 指针与函数
    void modify_value(int* p) {
        *p *= 2;
    }
    
    int value = 10;
    modify_value(&value);
    printf("\nAfter modify: value = %d\n", value); // 20
    
    // 指针数组
    const char* fruits[] = {"apple", "banana", "orange"};
    printf("\nPointer array:\n");
    for (int i = 0; i < 3; i++) {
        printf("%s\n", fruits[i]);
    }
    
    // 指向指针的指针
    int** pptr = &ptr;
    printf("\nPointer to pointer: **pptr = %d\n", **pptr); // 42
    
    // 空指针 (C11)
    int* null_ptr = NULL;
    if (null_ptr == NULL) {
        printf("null_ptr is NULL\n");
    }
    
    // 函数指针数组
    int (*operations[])(int, int) = {add_func, multiply_func};
    printf("\nFunction pointer array:\n");
    printf("operations[0](5, 3) = %d\n", operations[0](5, 3));
    printf("operations[1](5, 3) = %d\n", operations[1](5, 3));
    
    // void 指针
    void* vptr = &x;
    printf("\nvoid pointer: %d\n", *(int*)vptr);
    
    // const 指针
    int a = 10, b = 20;
    const int* cptr = &a;  // 指向常量的指针
    // *cptr = 20;  // 错误
    cptr = &b;  // 可以改变指向
    
    int* const pcptr = &a; // 指针常量
    *pcptr = 30;  // 可以改变值
    // pcptr = &b;  // 错误
    
    return 0;
}
```

#### 动态内存分配
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    // malloc
    int* arr1 = (int*)malloc(5 * sizeof(int));
    if (arr1 == NULL) {
        printf("Memory allocation failed\n");
        return 1;
    }
    
    for (int i = 0; i < 5; i++) {
        arr1[i] = (i + 1) * 10;
    }
    
    printf("malloc: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", arr1[i]);
    }
    printf("\n");
    free(arr1);
    
    // calloc（初始化为0）
    int* arr2 = (int*)calloc(5, sizeof(int));
    printf("calloc (all zeros): ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", arr2[i]);
    }
    printf("\n");
    free(arr2);
    
    // realloc
    int* arr3 = (int*)malloc(3 * sizeof(int));
    for (int i = 0; i < 3; i++) arr3[i] = i + 1;
    
    printf("Before realloc: ");
    for (int i = 0; i < 3; i++) printf("%d ", arr3[i]);
    printf("\n");
    
    arr3 = (int*)realloc(arr3, 6 * sizeof(int));
    for (int i = 3; i < 6; i++) arr3[i] = i + 1;
    
    printf("After realloc: ");
    for (int i = 0; i < 6; i++) printf("%d ", arr3[i]);
    printf("\n");
    free(arr3);
    
    // 动态分配字符串
    char* str = (char*)malloc(20);
    strcpy(str, "Hello, World!");
    printf("Dynamic string: %s\n", str);
    free(str);
    
    // 二维数组动态分配
    int rows = 3, cols = 4;
    int** matrix = (int**)malloc(rows * sizeof(int*));
    for (int i = 0; i < rows; i++) {
        matrix[i] = (int*)malloc(cols * sizeof(int));
    }
    
    // 填充矩阵
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            matrix[i][j] = i * cols + j + 1;
        }
    }
    
    // 打印矩阵
    printf("2D dynamic array:\n");
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            printf("%3d ", matrix[i][j]);
        }
        printf("\n");
    }
    
    // 释放二维数组
    for (int i = 0; i < rows; i++) {
        free(matrix[i]);
    }
    free(matrix);
    
    return 0;
}
```

### 数组与字符串

#### 数组操作
```c
#include <stdio.h>

int main() {
    // 一维数组
    int arr[5] = {1, 2, 3, 4, 5};
    
    // 遍历数组
    printf("Array: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
    
    // 数组长度计算
    size_t len = sizeof(arr) / sizeof(arr[0]);
    printf("Array length: %zu\n", len);
    
    // 数组求和
    int sum = 0;
    for (int i = 0; i < 5; i++) {
        sum += arr[i];
    }
    printf("Sum: %d\n", sum);
    
    // 查找最大值
    int max = arr[0];
    for (int i = 1; i < 5; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
    }
    printf("Max: %d\n", max);
    
    // 数组排序（冒泡排序）
    void bubble_sort(int* a, size_t n) {
        for (size_t i = 0; i < n - 1; i++) {
            for (size_t j = 0; j < n - i - 1; j++) {
                if (a[j] > a[j + 1]) {
                    int temp = a[j];
                    a[j] = a[j + 1];
                    a[j + 1] = temp;
                }
            }
        }
    }
    
    int unsorted[] = {5, 2, 8, 1, 9};
    bubble_sort(unsorted, 5);
    printf("Sorted: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", unsorted[i]);
    }
    printf("\n");
    
    // C99 变长数组
    int n = 5;
    int vla[n];
    for (int i = 0; i < n; i++) {
        vla[i] = i * 2;
    }
    printf("VLA: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", vla[i]);
    }
    printf("\n");
    
    // 二维数组
    int matrix[2][3] = {{1, 2, 3}, {4, 5, 6}};
    printf("2D array:\n");
    for (int i = 0; i < 2; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d ", matrix[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```

#### 字符串操作
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main() {
    // 字符串定义
    char str1[] = "Hello";
    char str2[] = "World";
    char str3[20];
    
    // 字符串长度
    printf("Length of '%s': %zu\n", str1, strlen(str1));
    
    // 字符串拷贝
    strcpy(str3, str1);
    printf("Copy: %s\n", str3);
    
    // 字符串连接
    strcat(str3, " ");
    strcat(str3, str2);
    printf("Concatenate: %s\n", str3);
    
    // 字符串比较
    int cmp = strcmp(str1, str2);
    printf("Compare: %d\n", cmp);
    
    // 字符串查找
    char* found = strstr(str3, "World");
    if (found != NULL) {
        printf("Found at position: %ld\n", found - str3);
    }
    
    // 字符大小写转换
    char text[] = "Hello World";
    for (int i = 0; text[i]; i++) {
        text[i] = toupper(text[i]);
    }
    printf("Uppercase: %s\n", text);
    
    for (int i = 0; text[i]; i++) {
        text[i] = tolower(text[i]);
    }
    printf("Lowercase: %s\n", text);
    
    // 字符串分割
    char sentence[] = "Hello,World,Programming,C";
    char* token = strtok(sentence, ",");
    printf("Tokens:\n");
    while (token != NULL) {
        printf("  %s\n", token);
        token = strtok(NULL, ",");
    }
    
    // 安全的字符串操作（C11）
    char dest[10];
    strncpy(dest, "Hello, World!", sizeof(dest) - 1);
    dest[sizeof(dest) - 1] = '\0';
    printf("Safe copy: %s\n", dest);
    
    // 字符数字转换
    char num_str[] = "12345";
    int num = atoi(num_str);
    printf("String to int: %d\n", num);
    
    char float_str[] = "3.14";
    double d = atof(float_str);
    printf("String to double: %f\n", d);
    
    char buffer[50];
    sprintf(buffer, "Value: %d", 42);
    printf("Sprintf: %s\n", buffer);
    
    // 字符统计
    char sample[] = "Hello123";
    int letters = 0, digits = 0;
    for (int i = 0; sample[i]; i++) {
        if (isalpha(sample[i])) letters++;
        if (isdigit(sample[i])) digits++;
    }
    printf("Letters: %d, Digits: %d\n", letters, digits);
    
    return 0;
}
```

### 结构体与联合体

#### 结构体
```c
#include <stdio.h>
#include <string.h>

// 结构体定义
typedef struct {
    char name[50];
    int age;
    float score;
} Student;

// 嵌套结构体
typedef struct {
    int day;
    int month;
    int year;
} Date;

typedef struct {
    char name[50];
    Date birthday;
} Person;

// 结构体数组
typedef struct {
    int x;
    int y;
} Point;

// 结构体指针
void print_student(const Student* s) {
    printf("Student: %s, Age: %d, Score: %.2f\n", 
           s->name, s->age, s->score);
}

// 函数返回结构体
Point create_point(int x, int y) {
    Point p = {x, y};
    return p;
}

int main() {
    // 结构体初始化
    Student s1 = {"Alice", 20, 95.5f};
    Student s2 = {.name = "Bob", .age = 21, .score = 88.0f};
    
    printf("Student 1: %s, %d, %.2f\n", s1.name, s1.age, s1.score);
    printf("Student 2: %s, %d, %.2f\n", s2.name, s2.age, s2.score);
    
    // 结构体赋值
    Student s3 = s1;
    strcpy(s3.name, "Charlie");
    printf("Student 3: %s, %d, %.2f\n", s3.name, s3.age, s3.score);
    
    // 结构体指针
    print_student(&s1);
    
    // 嵌套结构体
    Person p = {"John", {15, 6, 1990}};
    printf("Person: %s, Birthday: %d/%d/%d\n", 
           p.name, p.birthday.day, p.birthday.month, p.birthday.year);
    
    // 结构体数组
    Point points[] = {{0, 0}, {1, 1}, {2, 2}};
    printf("Points:\n");
    for (int i = 0; i < 3; i++) {
        printf("  (%d, %d)\n", points[i].x, points[i].y);
    }
    
    // 动态分配结构体
    Student* s_ptr = (Student*)malloc(sizeof(Student));
    if (s_ptr != NULL) {
        strcpy(s_ptr->name, "David");
        s_ptr->age = 22;
        s_ptr->score = 90.0f;
        print_student(s_ptr);
        free(s_ptr);
    }
    
    // 结构体对齐
    #pragma pack(push, 1)
    typedef struct {
        char c;
        int i;
    } PackedStruct;
    #pragma pack(pop)
    
    printf("Size of PackedStruct: %zu\n", sizeof(PackedStruct));
    
    // 结构体位域
    typedef struct {
        unsigned int flag1 : 1;
        unsigned int flag2 : 1;
        unsigned int value : 6;
    } BitField;
    
    BitField bf = {1, 0, 42};
    printf("BitField: %d, %d, %d\n", bf.flag1, bf.flag2, bf.value);
    
    return 0;
}
```

#### 联合体
```c
#include <stdio.h>

// 联合体定义
typedef union {
    int i;
    float f;
    char str[4];
} Data;

// 匿名联合体 (C11)
typedef struct {
    int type;  // 0: int, 1: float, 2: string
    union {
        int i;
        float f;
        char str[20];
    } value;
} Variant;

int main() {
    // 联合体示例
    Data data;
    data.i = 42;
    printf("As int: %d\n", data.i);
    
    data.f = 3.14f;
    printf("As float: %f\n", data.f);
    
    // 联合体共享内存
    printf("Raw bytes: ");
    for (int i = 0; i < 4; i++) {
        printf("%02x ", (unsigned char)data.str[i]);
    }
    printf("\n");
    
    // 匿名联合体
    Variant v1, v2, v3;
    
    v1.type = 0;
    v1.value.i = 100;
    printf("Variant int: %d\n", v1.value.i);
    
    v2.type = 1;
    v2.value.f = 3.14f;
    printf("Variant float: %f\n", v2.value.f);
    
    v3.type = 2;
    strcpy(v3.value.str, "Hello");
    printf("Variant string: %s\n", v3.value.str);
    
    // 联合体与结构体组合
    typedef struct {
        unsigned int is_int : 1;
        unsigned int is_float : 1;
        union {
            int i;
            float f;
        };
    } MultiType;
    
    MultiType mt;
    mt.is_int = 1;
    mt.is_float = 0;
    mt.i = 42;
    printf("MultiType: %d\n", mt.i);
    
    return 0;
}
```

### 预处理指令

#### 宏定义
```c
#include <stdio.h>

// 简单宏
#define PI 3.14159265
#define MAX_SIZE 100
#define DEBUG

// 带参数的宏
#define SQUARE(x) ((x) * (x))
#define MAX(a, b) ((a) > (b) ? (a) : (b))
#define SWAP(a, b) do { typeof(a) temp = a; a = b; b = temp; } while(0)

// 字符串化
#define STR(x) #x
#define CONCAT(a, b) a##b

// 条件编译
#ifdef DEBUG
    #define LOG(msg) printf("DEBUG: %s\n", msg)
#else
    #define LOG(msg)
#endif

// 多行宏
#define FOR_EACH_1(action, x) action(x)
#define FOR_EACH_2(action, x, ...) action(x); FOR_EACH_1(action, __VA_ARGS__)
#define FOR_EACH_3(action, x, ...) action(x); FOR_EACH_2(action, __VA_ARGS__)

// 调试宏
#define ASSERT(condition) \
    do { \
        if (!(condition)) { \
            fprintf(stderr, "Assertion failed: %s, file %s, line %d\n", \
                    #condition, __FILE__, __LINE__); \
            exit(1); \
        } \
    } while(0)

int main() {
    // 常量宏
    printf("PI = %f\n", PI);
    printf("MAX_SIZE = %d\n", MAX_SIZE);
    
    // 带参数的宏
    printf("SQUARE(5) = %d\n", SQUARE(5));
    printf("MAX(10, 20) = %d\n", MAX(10, 20));
    
    int a = 10, b = 20;
    printf("Before swap: a=%d, b=%d\n", a, b);
    SWAP(a, b);
    printf("After swap: a=%d, b=%d\n", a, b);
    
    // 字符串化
    printf("STR(Hello) = %s\n", STR(Hello));
    
    // 标记连接
    int CONCAT(var, 1) = 100;
    printf("var1 = %d\n", var1);
    
    // 条件编译
    LOG("This is a debug message");
    
    // 断言宏
    ASSERT(5 > 3);
    // ASSERT(3 > 5);  // 会触发断言失败
    
    // 预定义宏
    printf("File: %s\n", __FILE__);
    printf("Line: %d\n", __LINE__);
    printf("Function: %s\n", __func__);
    printf("Date: %s\n", __DATE__);
    printf("Time: %s\n", __TIME__);
    
    // 行号计算
    #define LINE_NUM __LINE__
    printf("Current line: %d\n", LINE_NUM);
    
    // 宏展开检测
    #ifdef MAX_SIZE
        printf("MAX_SIZE is defined\n");
    #endif
    
    // C11 静态断言
    #if __STDC_VERSION__ >= 201112L
        static_assert(sizeof(int) == 4, "int must be 4 bytes");
    #endif
    
    return 0;
}
```

### 文件操作

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    // 写入文件
    FILE* file = fopen("example.txt", "w");
    if (file == NULL) {
        printf("Cannot open file\n");
        return 1;
    }
    
    fprintf(file, "Hello, World!\n");
    fprintf(file, "This is a test file.\n");
    fclose(file);
    
    // 读取文件
    file = fopen("example.txt", "r");
    if (file == NULL) {
        printf("Cannot open file\n");
        return 1;
    }
    
    char buffer[256];
    printf("File content:\n");
    while (fgets(buffer, sizeof(buffer), file) != NULL) {
        printf("%s", buffer);
    }
    fclose(file);
    
    // 二进制文件操作
    int data[] = {10, 20, 30, 40, 50};
    file = fopen("data.bin", "wb");
    fwrite(data, sizeof(int), 5, file);
    fclose(file);
    
    int read_data[5];
    file = fopen("data.bin", "rb");
    fread(read_data, sizeof(int), 5, file);
    fclose(file);
    
    printf("Binary data: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", read_data[i]);
    }
    printf("\n");
    
    // 随机访问
    file = fopen("example.txt", "r");
    fseek(file, 0, SEEK_END);
    long file_size = ftell(file);
    printf("File size: %ld bytes\n", file_size);
    fclose(file);
    
    // 文件存在检查
    file = fopen("example.txt", "r");
    if (file != NULL) {
        printf("File exists\n");
        fclose(file);
    }
    
    // 临时文件
    FILE* temp = tmpfile();
    if (temp != NULL) {
        fprintf(temp, "Temporary data");
        rewind(temp);
        char temp_buffer[100];
        fgets(temp_buffer, sizeof(temp_buffer), temp);
        printf("Temp file: %s\n", temp_buffer);
        fclose(temp);
    }
    
    return 0;
}
```

### C11/C17/C23 新特性

#### C11 特性
```c
#include <stdio.h>
#include <threads.h>
#include <stdatomic.h>
#include <stdbool.h>
#include <assert.h>

// 线程函数
int thread_func(void* arg) {
    int id = *(int*)arg;
    printf("Thread %d running\n", id);
    return 0;
}

// 泛型选择
#define get_max(x, y) _Generic((x), \
    int: max_int, \
    double: max_double \
)(x, y)

int max_int(int a, int b) { return a > b ? a : b; }
double max_double(double a, double b) { return a > b ? a : b; }

int main() {
    // 泛型选择
    printf("Max int: %d\n", get_max(10, 20));
    printf("Max double: %f\n", get_max(3.14, 2.71));
    
    // 静态断言
    static_assert(sizeof(int) == 4, "int must be 4 bytes");
    printf("Static assertion passed\n");
    
    // 匿名结构体
    struct {
        int x, y;
    } point = {10, 20};
    printf("Point: (%d, %d)\n", point.x, point.y);
    
    // 原子操作
    atomic_int counter = ATOMIC_VAR_INIT(0);
    atomic_fetch_add(&counter, 1);
    printf("Atomic counter: %d\n", counter);
    
    // 线程
    int id = 1;
    thrd_t thread;
    thrd_create(&thread, thread_func, &id);
    thrd_join(thread, NULL);
    
    return 0;
}
```

## C++ 语法示例

### 基础语法

#### 类与对象
```cpp
#include <iostream>
#include <string>
#include <vector>

// 类定义
class Rectangle {
private:
    double width;
    double height;
    
public:
    // 构造函数
    Rectangle() : width(0), height(0) {}
    Rectangle(double w, double h) : width(w), height(h) {}
    
    // 拷贝构造函数
    Rectangle(const Rectangle& other) 
        : width(other.width), height(other.height) {
        std::cout << "Copy constructor called\n";
    }
    
    // 移动构造函数 (C++11)
    Rectangle(Rectangle&& other) noexcept
        : width(other.width), height(other.height) {
        std::cout << "Move constructor called\n";
        other.width = 0;
        other.height = 0;
    }
    
    // 析构函数
    ~Rectangle() {
        std::cout << "Destructor called\n";
    }
    
    // 成员函数
    double area() const {
        return width * height;
    }
    
    double perimeter() const {
        return 2 * (width + height);
    }
    
    // Getter 和 Setter
    double getWidth() const { return width; }
    double getHeight() const { return height; }
    
    void setWidth(double w) { width = w; }
    void setHeight(double h) { height = h; }
    
    // 静态成员函数
    static Rectangle createSquare(double side) {
        return Rectangle(side, side);
    }
    
    // 运算符重载
    Rectangle operator+(const Rectangle& other) const {
        return Rectangle(width + other.width, height + other.height);
    }
    
    bool operator==(const Rectangle& other) const {
        return width == other.width && height == other.height;
    }
    
    // 友元函数
    friend std::ostream& operator<<(std::ostream& os, const Rectangle& rect);
};

// 友元函数实现
std::ostream& operator<<(std::ostream& os, const Rectangle& rect) {
    os << "Rectangle(" << rect.width << " x " << rect.height << ")";
    return os;
}

int main() {
    // 创建对象
    Rectangle r1;
    Rectangle r2(5.0, 3.0);
    
    std::cout << "Area: " << r2.area() << std::endl;
    std::cout << "Perimeter: " << r2.perimeter() << std::endl;
    
    // 静态成员函数
    Rectangle square = Rectangle::createSquare(4.0);
    std::cout << "Square area: " << square.area() << std::endl;
    
    // 拷贝构造
    Rectangle r3 = r2;
    std::cout << "r3: " << r3 << std::endl;
    
    // 移动构造 (C++11)
    Rectangle r4 = std::move(r2);
    std::cout << "r4: " << r4 << std::endl;
    std::cout << "r2 after move: " << r2 << std::endl;
    
    // 运算符重载
    Rectangle r5 = r3 + r4;
    std::cout << "r3 + r4: " << r5 << std::endl;
    
    // 比较运算符
    if (r3 == r2) {
        std::cout << "Rectangles are equal\n";
    }
    
    return 0;
}
```

#### 继承与多态
```cpp
#include <iostream>
#include <vector>
#include <memory>

// 基类
class Shape {
public:
    virtual ~Shape() = default;
    virtual double area() const = 0;  // 纯虚函数
    virtual void draw() const { std::cout << "Drawing shape\n"; }
};

// 派生类
class Circle : public Shape {
private:
    double radius;
    
public:
    Circle(double r) : radius(r) {}
    
    double area() const override {
        return 3.14159 * radius * radius;
    }
    
    void draw() const override {
        std::cout << "Drawing circle with radius " << radius << std::endl;
    }
};

class Rectangle : public Shape {
private:
    double width;
    double height;
    
public:
    Rectangle(double w, double h) : width(w), height(h) {}
    
    double area() const override {
        return width * height;
    }
    
    void draw() const override {
        std::cout << "Drawing rectangle " << width << " x " << height << std::endl;
    }
};

// 抽象类
class Animal {
public:
    virtual ~Animal() = default;
    virtual void make_sound() const = 0;
};

class Dog : public Animal {
public:
    void make_sound() const override {
        std::cout << "Woof!\n";
    }
};

class Cat : public Animal {
public:
    void make_sound() const override {
        std::cout << "Meow!\n";
    }
};

// 多重继承
class Flyable {
public:
    virtual void fly() const { std::cout << "Flying\n"; }
};

class Bird : public Animal, public Flyable {
public:
    void make_sound() const override {
        std::cout << "Chirp!\n";
    }
    
    void fly() const override {
        std::cout << "Bird flying\n";
    }
};

int main() {
    // 多态
    std::vector<std::unique_ptr<Shape>> shapes;
    shapes.push_back(std::make_unique<Circle>(5.0));
    shapes.push_back(std::make_unique<Rectangle>(4.0, 3.0));
    
    for (const auto& shape : shapes) {
        shape->draw();
        std::cout << "Area: " << shape->area() << std::endl;
    }
    
    // 虚函数调用
    std::vector<std::unique_ptr<Animal>> animals;
    animals.push_back(std::make_unique<Dog>());
    animals.push_back(std::make_unique<Cat>());
    
    for (const auto& animal : animals) {
        animal->make_sound();
    }
    
    // 多重继承
    Bird bird;
    bird.make_sound();
    bird.fly();
    
    return 0;
}
```

#### 运算符重载
```cpp
#include <iostream>
#include <string>

class Complex {
private:
    double real;
    double imag;
    
public:
    Complex(double r = 0, double i = 0) : real(r), imag(i) {}
    
    // 算术运算符
    Complex operator+(const Complex& other) const {
        return Complex(real + other.real, imag + other.imag);
    }
    
    Complex operator-(const Complex& other) const {
        return Complex(real - other.real, imag - other.imag);
    }
    
    Complex operator*(const Complex& other) const {
        return Complex(real * other.real - imag * other.imag,
                      real * other.imag + imag * other.real);
    }
    
    Complex operator/(const Complex& other) const {
        double denom = other.real * other.real + other.imag * other.imag;
        return Complex((real * other.real + imag * other.imag) / denom,
                      (imag * other.real - real * other.imag) / denom);
    }
    
    // 复合赋值运算符
    Complex& operator+=(const Complex& other) {
        real += other.real;
        imag += other.imag;
        return *this;
    }
    
    Complex& operator-=(const Complex& other) {
        real -= other.real;
        imag -= other.imag;
        return *this;
    }
    
    // 关系运算符
    bool operator==(const Complex& other) const {
        return real == other.real && imag == other.imag;
    }
    
    bool operator!=(const Complex& other) const {
        return !(*this == other);
    }
    
    // 前置/后置自增自减
    Complex& operator++() {
        ++real;
        ++imag;
        return *this;
    }
    
    Complex operator++(int) {
        Complex temp = *this;
        ++(*this);
        return temp;
    }
    
    // 函数调用运算符
    double operator()(double x) const {
        return real * x + imag;
    }
    
    // 下标运算符
    double& operator[](int index) {
        return (index == 0) ? real : imag;
    }
    
    // 类型转换运算符
    operator double() const {
        return std::sqrt(real * real + imag * imag);
    }
    
    // 输入输出运算符（友元函数）
    friend std::ostream& operator<<(std::ostream& os, const Complex& c);
    friend std::istream& operator>>(std::istream& is, Complex& c);
};

std::ostream& operator<<(std::ostream& os, const Complex& c) {
    os << c.real;
    if (c.imag >= 0) os << "+";
    os << c.imag << "i";
    return os;
}

std::istream& operator>>(std::istream& is, Complex& c) {
    is >> c.real >> c.imag;
    return is;
}

int main() {
    Complex c1(3, 4);
    Complex c2(1, 2);
    
    std::cout << "c1 = " << c1 << std::endl;
    std::cout << "c2 = " << c2 << std::endl;
    
    // 算术运算
    Complex sum = c1 + c2;
    std::cout << "c1 + c2 = " << sum << std::endl;
    
    Complex diff = c1 - c2;
    std::cout << "c1 - c2 = " << diff << std::endl;
    
    Complex product = c1 * c2;
    std::cout << "c1 * c2 = " << product << std::endl;
    
    Complex quotient = c1 / c2;
    std::cout << "c1 / c2 = " << quotient << std::endl;
    
    // 复合赋值
    c1 += c2;
    std::cout << "c1 += c2, c1 = " << c1 << std::endl;
    
    // 自增
    ++c1;
    std::cout << "++c1 = " << c1 << std::endl;
    
    Complex c3 = c1++;
    std::cout << "c3 = c1++, c1 = " << c1 << ", c3 = " << c3 << std::endl;
    
    // 函数调用
    Complex c4(2, 3);
    std::cout << "c4(5) = " << c4(5) << std::endl;
    
    // 下标运算
    std::cout << "c4[0] = " << c4[0] << ", c4[1] = " << c4[1] << std::endl;
    
    // 类型转换
    std::cout << "Magnitude of c4 = " << static_cast<double>(c4) << std::endl;
    
    return 0;
}
```

### 模板编程

#### 函数模板
```cpp
#include <iostream>
#include <string>

// 基本函数模板
template <typename T>
T maximum(T a, T b) {
    return (a > b) ? a : b;
}

// 多参数模板
template <typename T, typename U>
auto add(T a, U b) -> decltype(a + b) {
    return a + b;
}

// 模板特化
template <>
const char* maximum<const char*>(const char* a, const char* b) {
    return (strcmp(a, b) > 0) ? a : b;
}

// 默认模板参数 (C++11)
template <typename T = int>
T multiply(T a, T b) {
    return a * b;
}

// 变参模板 (C++11)
template <typename... Args>
void print_all(Args... args) {
    (std::cout << ... << args) << std::endl;
}

// 折叠表达式 (C++17)
template <typename... Args>
auto sum_all(Args... args) {
    return (args + ... + 0);
}

// constexpr 模板 (C++11)
template <typename T>
constexpr T square(T x) {
    return x * x;
}

// 模板递归
template <int N>
struct Factorial {
    static constexpr int value = N * Factorial<N - 1>::value;
};

template <>
struct Factorial<0> {
    static constexpr int value = 1;
};

int main() {
    // 基本模板调用
    std::cout << "Maximum int: " << maximum(10, 20) << std::endl;
    std::cout << "Maximum double: " << maximum(3.14, 2.71) << std::endl;
    
    // 多参数模板
    std::cout << "Add: " << add(10, 3.5) << std::endl;
    
    // 模板特化
    std::cout << "Maximum string: " << maximum("Hello", "World") << std::endl;
    
    // 默认模板参数
    std::cout << "Multiply: " << multiply(5, 3) << std::endl;
    std::cout << "Multiply double: " << multiply(3.5, 2.0) << std::endl;
    
    // 变参模板
    print_all(1, 2, 3, 4, 5);
    print_all("Hello", " ", "World", "!");
    
    // 折叠表达式
    std::cout << "Sum: " << sum_all(1, 2, 3, 4, 5) << std::endl;
    
    // constexpr 模板
    constexpr int sq = square(5);
    std::cout << "Square of 5: " << sq << std::endl;
    
    // 编译期计算
    constexpr int fact = Factorial<5>::value;
    std::cout << "5! = " << fact << std::endl;
    
    return 0;
}
```

#### 类模板
```cpp
#include <iostream>
#include <vector>

// 基本类模板
template <typename T>
class Stack {
private:
    std::vector<T> elements;
    
public:
    void push(const T& element) {
        elements.push_back(element);
    }
    
    T pop() {
        if (elements.empty()) {
            throw std::runtime_error("Stack is empty");
        }
        T top = elements.back();
        elements.pop_back();
        return top;
    }
    
    T& top() {
        if (elements.empty()) {
            throw std::runtime_error("Stack is empty");
        }
        return elements.back();
    }
    
    bool empty() const {
        return elements.empty();
    }
    
    size_t size() const {
        return elements.size();
    }
};

// 部分特化
template <typename T>
class Stack<T*> {
private:
    std::vector<T*> elements;
    
public:
    void push(T* element) {
        elements.push_back(element);
    }
    
    T* pop() {
        if (elements.empty()) {
            throw std::runtime_error("Stack is empty");
        }
        T* top = elements.back();
        elements.pop_back();
        return top;
    }
};

// 非类型模板参数
template <typename T, size_t SIZE>
class FixedArray {
private:
    T array[SIZE];
    
public:
    T& operator[](size_t index) {
        return array[index];
    }
    
    const T& operator[](size_t index) const {
        return array[index];
    }
    
    size_t size() const {
        return SIZE;
    }
};

// 模板成员函数
template <typename T>
class Container {
private:
    std::vector<T> data;
    
public:
    template <typename U>
    void add(const U& item) {
        data.push_back(static_cast<T>(item));
    }
    
    void print() const {
        for (const auto& item : data) {
            std::cout << item << " ";
        }
        std::cout << std::endl;
    }
};

// CRTP 模式
template <typename Derived>
class Base {
public:
    void interface() {
        static_cast<Derived*>(this)->implementation();
    }
};

class Derived1 : public Base<Derived1> {
public:
    void implementation() {
        std::cout << "Derived1 implementation\n";
    }
};

class Derived2 : public Base<Derived2> {
public:
    void implementation() {
        std::cout << "Derived2 implementation\n";
    }
};

int main() {
    // 基本类模板
    Stack<int> int_stack;
    int_stack.push(10);
    int_stack.push(20);
    int_stack.push(30);
    
    std::cout << "Stack size: " << int_stack.size() << std::endl;
    std::cout << "Top element: " << int_stack.top() << std::endl;
    std::cout << "Popped: " << int_stack.pop() << std::endl;
    std::cout << "Stack size: " << int_stack.size() << std::endl;
    
    // 指针特化
    Stack<int*> ptr_stack;
    int a = 10, b = 20;
    ptr_stack.push(&a);
    ptr_stack.push(&b);
    std::cout << "Pointer stack top: " << *ptr_stack.top() << std::endl;
    
    // 非类型模板参数
    FixedArray<int, 5> fixed_array;
    for (size_t i = 0; i < fixed_array.size(); i++) {
        fixed_array[i] = i * 10;
    }
    std::cout << "Fixed array: ";
    for (size_t i = 0; i < fixed_array.size(); i++) {
        std::cout << fixed_array[i] << " ";
    }
    std::cout << std::endl;
    
    // 模板成员函数
    Container<double> container;
    container.add(10);      // int -> double
    container.add(3.5);     // double
    container.add(2.14f);   // float -> double
    std::cout << "Container: ";
    container.print();
    
    // CRTP 模式
    Derived1 d1;
    Derived2 d2;
    d1.interface();
    d2.interface();
    
    return 0;
}
```

### STL 容器与算法

#### 序列容器
```cpp
#include <iostream>
#include <vector>
#include <deque>
#include <list>
#include <forward_list>
#include <array>

int main() {
    // std::vector - 动态数组
    std::vector<int> vec;
    vec.push_back(1);
    vec.push_back(2);
    vec.push_back(3);
    vec.emplace_back(4);  // C++11: 就地构造
    
    std::cout << "Vector: ";
    for (int item : vec) {
        std::cout << item << " ";
    }
    std::cout << std::endl;
    
    std::cout << "Size: " << vec.size() << std::endl;
    std::cout << "Capacity: " << vec.capacity() << std::endl;
    vec.reserve(10);
    std::cout << "After reserve, capacity: " << vec.capacity() << std::endl;
    
    // std::array - 固定大小数组 (C++11)
    std::array<int, 5> arr = {1, 2, 3, 4, 5};
    std::cout << "Array: ";
    for (int item : arr) {
        std::cout << item << " ";
    }
    std::cout << std::endl;
    std::cout << "Array size: " << arr.size() << std::endl;
    
    // std::deque - 双端队列
    std::deque<int> dq;
    dq.push_back(1);
    dq.push_back(2);
    dq.push_front(0);
    dq.push_front(-1);
    
    std::cout << "Deque: ";
    for (int item : dq) {
        std::cout << item << " ";
    }
    std::cout << std::endl;
    
    // std::list - 双向链表
    std::list<int> lst = {1, 2, 3, 4, 5};
    lst.push_front(0);
    lst.push_back(6);
    lst.insert(++lst.begin(), 100);  // 在第二个位置插入
    
    std::cout << "List: ";
    for (int item : lst) {
        std::cout << item << " ";
    }
    std::cout << std::endl;
    
    // std::forward_list - 单向链表 (C++11)
    std::forward_list<int> flst = {1, 2, 3, 4, 5};
    flst.push_front(0);
    auto it = flst.begin();
    std::advance(it, 2);
    flst.insert_after(it, 100);  // 在第三个位置后插入
    
    std::cout << "Forward list: ";
    for (int item : flst) {
        std::cout << item << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

#### 关联容器
```cpp
#include <iostream>
#include <map>
#include <set>
#include <unordered_map>
#include <unordered_set>

int main() {
    // std::map - 有序映射
    std::map<std::string, int> word_count;
    word_count["apple"] = 5;
    word_count["banana"] = 3;
    word_count["orange"] = 2;
    
    std::cout << "Map:\n";
    for (const auto& pair : word_count) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    
    // std::unordered_map - 无序映射 (C++11)
    std::unordered_map<int, std::string> id_map;
    id_map[1] = "Alice";
    id_map[2] = "Bob";
    id_map[3] = "Charlie";
    
    std::cout << "\nUnordered map:\n";
    for (const auto& pair : id_map) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }
    
    // std::set - 有序集合
    std::set<int> numbers = {5, 2, 8, 1, 9};
    numbers.insert(3);
    numbers.insert(7);
    
    std::cout << "\nSet: ";
    for (int num : numbers) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // std::unordered_set - 无序集合 (C++11)
    std::unordered_set<std::string> unique_names = {"Alice", "Bob", "Alice", "Charlie"};
    std::cout << "\nUnordered set: ";
    for (const auto& name : unique_names) {
        std::cout << name << " ";
    }
    std::cout << std::endl;
    
    return 0;
}
```

#### STL 算法
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>
#include <functional>

int main() {
    std::vector<int> vec = {5, 2, 8, 1, 9, 3, 7, 4, 6};
    
    // 排序
    std::sort(vec.begin(), vec.end());
    std::cout << "Sorted: ";
    for (int num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // 稳定排序
    std::stable_sort(vec.begin(), vec.end());
    
    // 查找
    auto it = std::find(vec.begin(), vec.end(), 5);
    if (it != vec.end()) {
        std::cout << "Found 5 at position: " << (it - vec.begin()) << std::endl;
    }
    
    // 二分查找（需要有序）
    bool found = std::binary_search(vec.begin(), vec.end(), 7);
    std::cout << "Binary search 7: " << (found ? "Found" : "Not found") << std::endl;
    
    // 计数
    int count = std::count(vec.begin(), vec.end(), 5);
    std::cout << "Count of 5: " << count << std::endl;
    
    // 条件计数
    int even_count = std::count_if(vec.begin(), vec.end(), [](int x) { return x % 2 == 0; });
    std::cout << "Even numbers: " << even_count << std::endl;
    
    // 变换
    std::vector<int> result;
    std::transform(vec.begin(), vec.end(), std::back_inserter(result),
                   [](int x) { return x * 2; });
    std::cout << "Transformed (x2): ";
    for (int num : result) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // 求和
    int sum = std::accumulate(vec.begin(), vec.end(), 0);
    std::cout << "Sum: " << sum << std::endl;
    
    // 乘积
    int product = std::accumulate(vec.begin(), vec.end(), 1, std::multiplies<int>());
    std::cout << "Product: " << product << std::endl;
    
    // 最大最小值
    auto min_max = std::minmax_element(vec.begin(), vec.end());
    std::cout << "Min: " << *min_max.first << ", Max: " << *min_max.second << std::endl;
    
    // 去重
    std::vector<int> dup_vec = {1, 2, 2, 3, 3, 3, 4, 5, 5};
    std::sort(dup_vec.begin(), dup_vec.end());
    auto last = std::unique(dup_vec.begin(), dup_vec.end());
    dup_vec.erase(last, dup_vec.end());
    std::cout << "Unique: ";
    for (int num : dup_vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // 复制
    std::vector<int> copy_vec;
    std::copy(vec.begin(), vec.end(), std::back_inserter(copy_vec));
    
    // 遍历
    std::for_each(vec.begin(), vec.end(), [](int x) {
        std::cout << x << " ";
    });
    std::cout << std::endl;
    
    // 并行算法 (C++17)
    #include <execution>
    std::vector<int> large_vec(1000000);
    std::iota(large_vec.begin(), large_vec.end(), 0);  // 填充 0, 1, 2, ..., 999999
    std::sort(std::execution::par, large_vec.begin(), large_vec.end());
    
    return 0;
}
```

### 智能指针与 RAII

#### 智能指针
```cpp
#include <iostream>
#include <memory>
#include <vector>

// 用于演示的类
class Resource {
private:
    int id;
    
public:
    Resource(int id) : id(id) {
        std::cout << "Resource " << id << " created\n";
    }
    
    ~Resource() {
        std::cout << "Resource " << id << " destroyed\n";
    }
    
    void doSomething() {
        std::cout << "Resource " << id << " working\n";
    }
};

int main() {
    // std::unique_ptr - 独占所有权
    std::cout << "=== unique_ptr ===" << std::endl;
    {
        std::unique_ptr<Resource> ptr1(new Resource(1));
        ptr1->doSomething();
        
        // make_unique (C++14)
        auto ptr2 = std::make_unique<Resource>(2);
        ptr2->doSomething();
        
        // 所有权转移
        std::unique_ptr<Resource> ptr3 = std::move(ptr1);
        if (ptr1 == nullptr) {
            std::cout << "ptr1 is null after move\n";
        }
        ptr3->doSomething();
        
        // 自定义删除器
        auto deleter = [](Resource* r) {
            std::cout << "Custom deleter called\n";
            delete r;
        };
        std::unique_ptr<Resource, decltype(deleter)> ptr4(new Resource(4), deleter);
    }
    std::cout << "unique_ptr scope ended\n\n";
    
    // std::shared_ptr - 共享所有权
    std::cout << "=== shared_ptr ===" << std::endl;
    {
        auto shared1 = std::make_shared<Resource>(5);
        std::cout << "Use count: " << shared1.use_count() << std::endl;
        
        auto shared2 = shared1;
        std::cout << "Use count: " << shared1.use_count() << std::endl;
        
        {
            auto shared3 = shared2;
            std::cout << "Use count: " << shared1.use_count() << std::endl;
        }
        std::cout << "After shared3 destruction, use count: " 
                  << shared1.use_count() << std::endl;
    }
    std::cout << "shared_ptr scope ended\n\n";
    
    // std::weak_ptr - 避免循环引用
    std::cout << "=== weak_ptr ===" << std::endl;
    {
        auto shared = std::make_shared<Resource>(6);
        std::weak_ptr<Resource> weak = shared;
        
        std::cout << "Weak use count: " << weak.use_count() << std::endl;
        
        if (auto locked = weak.lock()) {
            locked->doSomething();
        }
        
        shared.reset();
        if (weak.expired()) {
            std::cout << "Weak pointer expired\n";
        }
    }
    std::cout << "weak_ptr scope ended\n\n";
    
    // 智能指针与容器
    std::cout << "=== smart pointers in container ===" << std::endl;
    std::vector<std::unique_ptr<Resource>> resources;
    resources.push_back(std::make_unique<Resource>(7));
    resources.push_back(std::make_unique<Resource>(8));
    
    for (const auto& res : resources) {
        res->doSomething();
    }
    
    return 0;
}
```

#### RAII 模式
```cpp
#include <iostream>
#include <fstream>
#include <memory>

// 文件句柄 RAII 包装
class FileHandler {
private:
    FILE* file;
    
public:
    FileHandler(const char* filename, const char* mode) {
        file = fopen(filename, mode);
        if (!file) {
            throw std::runtime_error("Cannot open file");
        }
        std::cout << "File opened: " << filename << std::endl;
    }
    
    ~FileHandler() {
        if (file) {
            fclose(file);
            std::cout << "File closed" << std::endl;
        }
    }
    
    // 禁止拷贝
    FileHandler(const FileHandler&) = delete;
    FileHandler& operator=(const FileHandler&) = delete;
    
    // 允许移动
    FileHandler(FileHandler&& other) noexcept : file(other.file) {
        other.file = nullptr;
    }
    
    FILE* get() const { return file; }
    
    void write(const char* text) {
        fprintf(file, "%s", text);
    }
    
    std::string read() {
        fseek(file, 0, SEEK_END);
        long size = ftell(file);
        fseek(file, 0, SEEK_SET);
        
        std::string content(size, '\0');
        fread(&content[0], 1, size, file);
        return content;
    }
};

// 锁的 RAII 包装
#include <mutex>
class ScopedLock {
private:
    std::mutex& mtx;
    
public:
    ScopedLock(std::mutex& m) : mtx(m) {
        mtx.lock();
        std::cout << "Lock acquired" << std::endl;
    }
    
    ~ScopedLock() {
        mtx.unlock();
        std::cout << "Lock released" << std::endl;
    }
    
    // 禁止拷贝和移动
    ScopedLock(const ScopedLock&) = delete;
    ScopedLock& operator=(const ScopedLock&) = delete;
};

// 使用 RAII 的示例
int main() {
    // 文件处理 RAII
    try {
        FileHandler file("test.txt", "w");
        file.write("Hello, RAII!");
        // 文件会自动关闭
    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }
    
    // 互斥锁 RAII
    std::mutex mtx;
    {
        ScopedLock lock(mtx);
        std::cout << "Critical section" << std::endl;
        // 锁会自动释放
    }
    
    // 标准库使用 RAII
    std::ofstream outfile("example.txt");
    outfile << "Auto-close example";
    // 析构时自动关闭
    
    return 0;
}
```

### Lambda 表达式

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>

int main() {
    // 基本 Lambda
    auto greet = []() {
        std::cout << "Hello, Lambda!" << std::endl;
    };
    greet();
    
    // 带参数的 Lambda
    auto add = [](int a, int b) {
        return a + b;
    };
    std::cout << "5 + 3 = " << add(5, 3) << std::endl;
    
    // 捕获外部变量
    int factor = 2;
    auto multiply = [factor](int x) {
        return x * factor;
    };
    std::cout << "5 * 2 = " << multiply(5) << std::endl;
    
    // 捕获引用
    int sum = 0;
    auto accumulate = [&sum](int x) {
        sum += x;
    };
    accumulate(10);
    accumulate(20);
    std::cout << "Sum: " << sum << std::endl;
    
    // 混合捕获
    int multiplier = 3;
    int total = 0;
    auto mixed_capture = [multiplier, &total](int x) {
        total += x * multiplier;
    };
    mixed_capture(5);
    mixed_capture(7);
    std::cout << "Total: " << total << std::endl;
    
    // 可变 Lambda (C++11)
    auto mutable_lambda = [factor]() mutable {
        factor *= 2;
        return factor;
    };
    std::cout << "Mutable: " << mutable_lambda() << std::endl;
    
    // Lambda 作为算法参数
    std::vector<int> vec = {5, 2, 8, 1, 9};
    std::sort(vec.begin(), vec.end());
    std::cout << "Sorted: ";
    for (int num : vec) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // 条件查找
    auto even = [](int x) { return x % 2 == 0; };
    auto it = std::find_if(vec.begin(), vec.end(), even);
    if (it != vec.end()) {
        std::cout << "First even: " << *it << std::endl;
    }
    
    // 变换
    std::vector<int> doubled;
    std::transform(vec.begin(), vec.end(), std::back_inserter(doubled),
                   [](int x) { return x * 2; });
    std::cout << "Doubled: ";
    for (int num : doubled) {
        std::cout << num << " ";
    }
    std::cout << std::endl;
    
    // Lambda 返回类型
    auto divide = [](int a, int b) -> double {
        return static_cast<double>(a) / b;
    };
    std::cout << "10 / 3 = " << divide(10, 3) << std::endl;
    
    // 泛型 Lambda (C++14)
    auto generic_add = [](auto a, auto b) {
        return a + b;
    };
    std::cout << "Generic add int: " << generic_add(5, 3) << std::endl;
    std::cout << "Generic add double: " << generic_add(3.14, 2.86) << std::endl;
    
    // Lambda 捕获初始化 (C++14)
    auto capture_init = [value = std::string("Hello")]() {
        return value;
    };
    std::cout << "Capture init: " << capture_init() << std::endl;
    
    // constexpr Lambda (C++17)
    constexpr auto square = [](int x) {
        return x * x;
    };
    constexpr int sq = square(5);
    std::cout << "constexpr square: " << sq << std::endl;
    
    return 0;
}
```

### 并发编程

#### 线程与同步
```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>
#include <future>
#include <atomic>
#include <chrono>

std::mutex mtx;
int shared_counter = 0;

// 线程函数
void worker(int id) {
    for (int i = 0; i < 3; i++) {
        std::lock_guard<std::mutex> lock(mtx);
        shared_counter++;
        std::cout << "Worker " << id << ": counter = " << shared_counter << std::endl;
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
    }
}

// 条件变量示例
std::mutex cv_mtx;
std::condition_variable cv;
bool ready = false;
int data = 0;

void producer() {
    std::this_thread::sleep_for(std::chrono::seconds(1));
    
    {
        std::lock_guard<std::mutex> lock(cv_mtx);
        data = 42;
        ready = true;
        std::cout << "Producer: data = " << data << std::endl;
    }
    cv.notify_one();
}

void consumer() {
    std::unique_lock<std::mutex> lock(cv_mtx);
    cv.wait(lock, [] { return ready; });
    std::cout << "Consumer: data = " << data << std::endl;
}

// Future 和 Promise
int async_task(int x) {
    std::this_thread::sleep_for(std::chrono::seconds(2));
    return x * x;
}

// 原子操作
std::atomic<int> atomic_counter{0};

void atomic_worker() {
    for (int i = 0; i < 1000; i++) {
        atomic_counter.fetch_add(1, std::memory_order_relaxed);
    }
}

int main() {
    // 基本线程
    std::cout << "=== Basic Threads ===" << std::endl;
    std::thread t1(worker, 1);
    std::thread t2(worker, 2);
    
    t1.join();
    t2.join();
    std::cout << "Final counter: " << shared_counter << std::endl << std::endl;
    
    // Lambda 线程
    std::cout << "=== Lambda Threads ===" << std::endl;
    std::thread t3([] {
        std::cout << "Lambda thread running" << std::endl;
    });
    t3.join();
    
    // 条件变量
    std::cout << "=== Condition Variable ===" << std::endl;
    std::thread prod(producer);
    std::thread cons(consumer);
    prod.join();
    cons.join();
    
    // Future 和 async
    std::cout << "\n=== Future and Async ===" << std::endl;
    auto future = std::async(std::launch::async, async_task, 10);
    std::cout << "Waiting for result..." << std::endl;
    int result = future.get();
    std::cout << "Result: " << result << std::endl;
    
    // Promise
    std::promise<int> p;
    auto f = p.get_future();
    
    std::thread setter([&p] {
        std::this_thread::sleep_for(std::chrono::seconds(1));
        p.set_value(100);
    });
    
    std::cout << "Promise value: " << f.get() << std::endl;
    setter.join();
    
    // 原子操作
    std::cout << "\n=== Atomic Operations ===" << std::endl;
    std::vector<std::thread> atomic_workers;
    for (int i = 0; i < 10; i++) {
        atomic_workers.emplace_back(atomic_worker);
    }
    for (auto& t : atomic_workers) {
        t.join();
    }
    std::cout << "Atomic counter: " << atomic_counter << std::endl;
    
    // std::jthread (C++20)
    #if __cplusplus >= 202002L
    std::cout << "\n=== JThread (C++20) ===" << std::endl;
    std::jthread jt([] {
        std::cout << "JThread running" << std::endl;
    });
    // 自动 join
    #endif
    
    return 0;
}
```

## 常见模式与最佳实践

### 设计模式实现

#### 单例模式
```cpp
#include <iostream>
#include <memory>

class Singleton {
private:
    static std::unique_ptr<Singleton> instance;
    static std::mutex mtx;
    
    Singleton() {
        std::cout << "Singleton created" << std::endl;
    }
    
public:
    // 禁止拷贝和移动
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;
    Singleton(Singleton&&) = delete;
    Singleton& operator=(Singleton&&) = delete;
    
    static Singleton& getInstance() {
        std::lock_guard<std::mutex> lock(mtx);
        if (!instance) {
            instance = std::unique_ptr<Singleton>(new Singleton());
        }
        return *instance;
    }
    
    void doSomething() {
        std::cout << "Singleton doing something" << std::endl;
    }
};

// 静态成员初始化
std::unique_ptr<Singleton> Singleton::instance = nullptr;
std::mutex Singleton::mtx;

// 更简单的单例（Meyers' Singleton）
class SimpleSingleton {
private:
    SimpleSingleton() {
        std::cout << "SimpleSingleton created" << std::endl;
    }
    
public:
    static SimpleSingleton& getInstance() {
        static SimpleSingleton instance;
        return instance;
    }
    
    void doSomething() {
        std::cout << "SimpleSingleton doing something" << std::endl;
    }
};

int main() {
    // 双重检查锁定
    auto& singleton = Singleton::getInstance();
    singleton.doSomething();
    
    // Meyers' Singleton
    auto& simple = SimpleSingleton::getInstance();
    simple.doSomething();
    
    return 0;
}
```

#### 工厂模式
```cpp
#include <iostream>
#include <memory>
#include <string>

// 产品接口
class Product {
public:
    virtual ~Product() = default;
    virtual void use() = 0;
};

// 具体产品
class ConcreteProductA : public Product {
public:
    void use() override {
        std::cout << "Using Product A" << std::endl;
    }
};

class ConcreteProductB : public Product {
public:
    void use() override {
        std::cout << "Using Product B" << std::endl;
    }
};

// 工厂
class Factory {
public:
    enum class ProductType { A, B };
    
    static std::unique_ptr<Product> createProduct(ProductType type) {
        switch (type) {
            case ProductType::A:
                return std::make_unique<ConcreteProductA>();
            case ProductType::B:
                return std::make_unique<ConcreteProductB>();
            default:
                throw std::invalid_argument("Unknown product type");
        }
    }
};

int main() {
    auto productA = Factory::createProduct(Factory::ProductType::A);
    productA->use();
    
    auto productB = Factory::createProduct(Factory::ProductType::B);
    productB->use();
    
    return 0;
}
```

#### 观察者模式
```cpp
#include <iostream>
#include <vector>
#include <memory>
#include <functional>

// 观察者接口
class Observer {
public:
    virtual ~Observer() = default;
    virtual void update(const std::string& message) = 0;
};

// 具体观察者
class ConcreteObserver : public Observer {
private:
    std::string name;
    
public:
    ConcreteObserver(const std::string& name) : name(name) {}
    
    void update(const std::string& message) override {
        std::cout << name << " received: " << message << std::endl;
    }
};

// 被观察者
class Subject {
private:
    std::vector<std::weak_ptr<Observer>> observers;
    std::mutex mtx;
    
public:
    void attach(const std::shared_ptr<Observer>& observer) {
        std::lock_guard<std::mutex> lock(mtx);
        observers.push_back(observer);
    }
    
    void notify(const std::string& message) {
        std::lock_guard<std::mutex> lock(mtx);
        auto it = observers.begin();
        while (it != observers.end()) {
            auto observer = it->lock();
            if (observer) {
                observer->update(message);
                ++it;
            } else {
                it = observers.erase(it);
            }
        }
    }
};

int main() {
    Subject subject;
    
    auto obs1 = std::make_shared<ConcreteObserver>("Observer 1");
    auto obs2 = std::make_shared<ConcreteObserver>("Observer 2");
    auto obs3 = std::make_shared<ConcreteObserver>("Observer 3");
    
    subject.attach(obs1);
    subject.attach(obs2);
    subject.attach(obs3);
    
    subject.notify("Hello, observers!");
    
    obs2.reset();  // 移除一个观察者
    subject.notify("Another message");
    
    return 0;
}
```

## 性能优化示例

### 移动语义优化
```cpp
#include <iostream>
#include <vector>
#include <string>

class BigData {
private:
    std::string data;
    int* array;
    size_t size;
    
public:
    BigData(const std::string& str, size_t n) 
        : data(str), size(n) {
        array = new int[size];
        std::cout << "BigData created: " << data << std::endl;
    }
    
    // 拷贝构造
    BigData(const BigData& other) 
        : data(other.data), size(other.size) {
        array = new int[size];
        std::copy(other.array, other.array + size, array);
        std::cout << "BigData copied: " << data << std::endl;
    }
    
    // 移动构造
    BigData(BigData&& other) noexcept
        : data(std::move(other.data)), 
          array(other.array), 
          size(other.size) {
        other.array = nullptr;
        other.size = 0;
        std::cout << "BigData moved: " << data << std::endl;
    }
    
    ~BigData() {
        delete[] array;
    }
    
    BigData& operator=(const BigData& other) {
        if (this != &other) {
            delete[] array;
            data = other.data;
            size = other.size;
            array = new int[size];
            std::copy(other.array, other.array + size, array);
        }
        return *this;
    }
    
    BigData& operator=(BigData&& other) noexcept {
        if (this != &other) {
            delete[] array;
            data = std::move(other.data);
            array = other.array;
            size = other.size;
            other.array = nullptr;
            other.size = 0;
        }
        return *this;
    }
};

int main() {
    // 不使用移动语义（会拷贝）
    std::cout << "=== Without move semantics ===" << std::endl;
    std::vector<BigData> vec1;
    vec1.push_back(BigData("Data1", 100));  // 临时对象，可能被优化
    
    // 使用移动语义
    std::cout << "\n=== With move semantics ===" << std::endl;
    BigData data2("Data2", 100);
    vec1.push_back(std::move(data2));  // 明确移动
    
    // emplace_back (C++11) - 就地构造
    std::cout << "\n=== emplace_back ===" << std::endl;
    vec1.emplace_back("Data3", 100);  // 直接构造，无需临时对象
    
    return 0;
}
```

### 内存预分配
```cpp
#include <iostream>
#include <vector>

int main() {
    // 不预分配（可能多次重分配）
    std::cout << "=== Without preallocation ===" << std::endl;
    std::vector<int> vec1;
    for (int i = 0; i < 1000; i++) {
        vec1.push_back(i);
        if (vec1.capacity() == vec1.size()) {
            std::cout << "Reallocation at size: " << vec1.size() << std::endl;
        }
    }
    
    // 预分配
    std::cout << "\n=== With preallocation ===" << std::endl;
    std::vector<int> vec2;
    vec2.reserve(1000);
    std::cout << "Initial capacity: " << vec2.capacity() << std::endl;
    for (int i = 0; i < 1000; i++) {
        vec2.push_back(i);
    }
    std::cout << "Final capacity: " << vec2.capacity() << std::endl;
    std::cout << "Size: " << vec2.size() << std::endl;
    
    return 0;
}
```

## 错误处理示例

### 异常处理
```cpp
#include <iostream>
#include <stdexcept>
#include <string>

class CustomException : public std::exception {
private:
    std::string message;
    
public:
    CustomException(const std::string& msg) : message(msg) {}
    
    const char* what() const noexcept override {
        return message.c_str();
    }
};

void risky_function(int value) {
    if (value < 0) {
        throw std::invalid_argument("Value must be positive");
    }
    if (value == 0) {
        throw CustomException("Value cannot be zero");
    }
    std::cout << "Processing value: " << value << std::endl;
}

int main() {
    try {
        risky_function(10);
        risky_function(0);  // 抛出异常
    } catch (const std::invalid_argument& e) {
        std::cerr << "Invalid argument: " << e.what() << std::endl;
    } catch (const CustomException& e) {
        std::cerr << "Custom exception: " << e.what() << std::endl;
    } catch (const std::exception& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    } catch (...) {
        std::cerr << "Unknown exception" << std::endl;
    }
    
    // noexcept 函数
    auto safe_function = []() noexcept -> int {
        return 42;
    };
    
    return 0;
}
```

### 错误码方式
```cpp
#include <iostream>
#include <string>

enum class ErrorCode {
    Success = 0,
    InvalidInput,
    FileNotFound,
    PermissionDenied
};

Result divide(int a, int b, ErrorCode& error) {
    if (b == 0) {
        error = ErrorCode::InvalidInput;
        return 0;
    }
    error = ErrorCode::Success;
    return a / b;
}

int main() {
    ErrorCode error;
    int result = divide(10, 0, error);
    
    if (error == ErrorCode::Success) {
        std::cout << "Result: " << result << std::endl;
    } else {
        std::cout << "Error occurred" << std::endl;
    }
    
    return 0;
}
```

### 使用 optional (C++17)
```cpp
#include <iostream>
#include <optional>
#include <string>

std::optional<int> divide(int a, int b) {
    if (b == 0) {
        return std::nullopt;
    }
    return a / b;
}

std::optional<int> parse_int(const std::string& str) {
    try {
        return std::stoi(str);
    } catch (...) {
        return std::nullopt;
    }
}

int main() {
    // optional 示例
    auto result = divide(10, 2);
    if (result) {
        std::cout << "Division result: " << *result << std::endl;
    } else {
        std::cout << "Division by zero" << std::endl;
    }
    
    // optional 的 or_else
    auto value = parse_int("invalid").value_or(0);
    std::cout << "Parsed value: " << value << std::endl;
    
    // optional 的 transform
    auto result2 = divide(10, 2).transform([](int x) { return x * 2; });
    if (result2) {
        std::cout << "Doubled result: " << *result2 << std::endl;
    }
    
    return 0;
}
```

这个文档包含了大量的 C/C++ 语法示例，涵盖了从基础语法到高级特性的各个方面，可以作为学习和参考的实用指南。
