# C/C++ Standard Library Reference

## Table of Contents
- [C Standard Library](#c-standard-library)
- [C++ Standard Library](#c-standard-library-1)
- [Version Differences](#version-differences)
- [Best Practices](#best-practices)

## C Standard Library

### Standard Headers

#### Input/Output
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<stdio.h>` | Standard input/output | printf, scanf, fopen, fread, fwrite |
| `<wchar.h>` | Wide character input/output | wprintf, wscanf, fwprintf |

#### Character Handling
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<ctype.h>` | Character classification and conversion | isalpha, isdigit, toupper, tolower |
| `<wctype.h>` | Wide character classification | iswalpha, iswdigit, towupper, towlower |

#### String Handling
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<string.h>` | String operations | strcpy, strcmp, strlen, strstr |
| `<wchar.h>` | Wide string operations | wcscpy, wcscmp, wcslen |

#### Math Functions
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<math.h>` | Mathematical operations | sin, cos, sqrt, pow, exp |
| `<complex.h>` | Complex number operations | creal, cimag, cexp, csqrt |
| `<fenv.h>` | Floating-point environment | fegetround, fesetround |

#### Utility Functions
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<stdlib.h>` | General utilities | malloc, free, atoi, rand, qsort |
| `<time.h>` | Time handling | time, localtime, strftime |

#### Diagnostics
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<assert.h>` | Assertions | assert |
| `<errno.h>` | Error codes | errno |

#### Variable Arguments
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<stdarg.h>` | Variable arguments | va_start, va_arg, va_end |
| `<stdargs.h>` | Same as above (old name) | Same as above |

#### Memory Management
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<stdlib.h>` | Dynamic memory | malloc, calloc, realloc, free |
| `<string.h>` | Memory operations | memcpy, memset, memcmp |

#### Signal Handling
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<signal.h>` | Signal handling | signal, raise |
| `<setjmp.h>` | Non-local jumps | setjmp, longjmp |

#### Localization
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<locale.h>` | Localization | setlocale, localeconv |

#### Data Types
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<stddef.h>` | Standard definitions | size_t, ptrdiff_t, NULL, offsetof |
| `<stdint.h>` | Standard integers | int32_t, uint64_t, INT32_MAX |
| `<stdbool.h>` | Boolean type | bool, true, false (C99) |
| `<inttypes.h>` | Integer formatting | PRId32, SCNu64 |

#### Date and Time
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<time.h>` | Time handling | time, clock, difftime |

#### Atomic Operations (C11)
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<stdatomic.h>` | Atomic operations | atomic_load, atomic_store, atomic_fetch_add |

#### Thread Support (C11)
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<threads.h>` | Thread management | thrd_create, thrd_join, mtx_lock, cnd_wait |
| `<time.h>` | Thread time | thrd_sleep |

#### Generic Types (C11)
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<stdalign.h>` | Alignment | alignas, alignof |
| `<stdnoreturn.h>` | Non-returning functions | noreturn |

#### Bounds Checking Functions (C11)
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<stdckdint.h>` | Overflow checking | ckd_add, ckd_mul, ckd_sub |

### C Standard Library Detailed Reference

#### 1. Input/Output Library (`<stdio.h>`)

**File Operation Functions**

| Function | Description | Return Value |
|----------|-------------|--------------|
| `fopen()` | Open a file | FILE* or NULL |
| `fclose()` | Close a file | 0 (success) or EOF |
| `fflush()` | Flush buffer | 0 (success) or EOF |
| `fseek()` | Set file position | 0 (success) or non-zero |
| `ftell()` | Get current position | Current position or -1L |
| `rewind()` | Reset file position to beginning | None |
| `fgetpos()` | Get file position | 0 (success) or non-zero |
| `fsetpos()` | Set file position | 0 (success) or non-zero |
| `remove()` | Delete a file | 0 (success) or non-zero |
| `rename()` | Rename a file | 0 (success) or non-zero |
| `tmpfile()` | Create a temporary file | FILE* or NULL |
| `tmpnam()` | Generate a temporary filename | Filename or NULL |

**Formatted Output**

| Function | Description | Example |
|----------|-------------|---------|
| `printf()` | Formatted output to stdout | `printf("Value: %d\n", x)` |
| `fprintf()` | Formatted output to stream | `fprintf(fp, "Data: %f\n", y)` |
| `sprintf()` | Formatted output to string | `sprintf(buf, "Result: %s", str)` |
| `snprintf()` | Formatted output to string (length-limited) | `snprintf(buf, 10, "Val: %d", x)` |

**Formatted Input**

| Function | Description | Example |
|----------|-------------|---------|
| `scanf()` | Read from stdin | `scanf("%d", &value)` |
| `fscanf()` | Read from stream | `fscanf(fp, "%f", &num)` |
| `sscanf()` | Read from string | `sscanf(str, "%d-%d", &a, &b)` |

**Character Input/Output**

| Function | Description |
|----------|-------------|
| `fputc()` | Output character to stream |
| `putc()` | Macro version of fputc |
| `putchar()` | Output character to stdout |
| `fputs()` | Output string to stream |
| `puts()` | Output string to stdout |
| `fgetc()` | Read character from stream |
| `getc()` | Macro version of fgetc |
| `getchar()` | Read character from stdin |
| `fgets()` | Read string from stream |
| `gets()` | Read string from stdin (deprecated) |

**Block Input/Output**

| Function | Description |
|----------|-------------|
| `fread()` | Read data block from stream |
| `fwrite()` | Write data block to stream |

**File Status**

| Function | Description |
|----------|-------------|
| `feof()` | Check end-of-file flag |
| `ferror()` | Check error flag |
| `clearerr()` | Clear error and end-of-file flags |

#### 2. String Library (`<string.h>`)

**String Operations**

| Function | Description | Notes |
|----------|-------------|-------|
| `strcpy()` | Copy string | Potential buffer overflow |
| `strncpy()` | Copy specified length | Does not guarantee null termination |
| `strcat()` | Concatenate strings | Potential buffer overflow |
| `strncat()` | Concatenate specified length | Guarantees null termination |
| `strcmp()` | Compare strings | Returns negative/0/positive |
| `strncmp()` | Compare specified length | Same as above |
| `strlen()` | Calculate length | Excludes null character |

**String Search**

| Function | Description | Return Value |
|----------|-------------|--------------|
| `strchr()` | Find character | Pointer if found, NULL if not |
| `strrchr()` | Find character from end | Same as above |
| `strstr()` | Find substring | Same as above |
| `strpbrk()` | Find any matching character | Same as above |
| `strspn()` | Count leading matching characters | Returns length |
| `strcspn()` | Count leading non-matching characters | Returns length |
| `strtok()` | Tokenize string | Requires multiple calls |

**Memory Operations**

| Function | Description | Usage |
|----------|-------------|-------|
| `memcpy()` | Copy memory block | Copy non-overlapping regions |
| `memmove()` | Copy memory block | Handles overlapping regions |
| `memset()` | Set memory value | Initialize memory |
| `memcmp()` | Compare memory blocks | Compare binary data |
| `memchr()` | Find character in memory | Find byte |

#### 3. Math Library (`<math.h>`)

**Trigonometric Functions**

| Function | Description | Example |
|----------|-------------|---------|
| `sin()` | Sine | `sin(PI/2) = 1` |
| `cos()` | Cosine | `cos(0) = 1` |
| `tan()` | Tangent | `tan(PI/4) = 1` |
| `asin()` | Arcsine | `asin(0.5) = PI/6` |
| `acos()` | Arccosine | `acos(0) = PI/2` |
| `atan()` | Arctangent | `atan(1) = PI/4` |
| `atan2()` | Arctangent(y, x) | `atan2(1, 1) = PI/4` |

**Hyperbolic Functions**

| Function | Description |
|----------|-------------|
| `sinh()` | Hyperbolic sine |
| `cosh()` | Hyperbolic cosine |
| `tanh()` | Hyperbolic tangent |

**Exponential and Logarithmic**

| Function | Description | Example |
|----------|-------------|---------|
| `exp()` | Power of e | `exp(1) = e` |
| `log()` | Natural logarithm | `log(e) = 1` |
| `log10()` | Base 10 logarithm | `log10(100) = 2` |
| `log2()` | Base 2 logarithm | `log2(8) = 3` |

**Power Functions**

| Function | Description |
|----------|-------------|
| `pow()` | Exponentiation |
| `sqrt()` | Square root |
| `cbrt()` | Cube root |

**Rounding Functions**

| Function | Description | Example |
|----------|-------------|---------|
| `ceil()` | Round up | `ceil(2.3) = 3` |
| `floor()` | Round down | `floor(2.7) = 2` |
| `round()` | Round to nearest | `round(2.5) = 3` |
| `trunc()` | Truncate | `trunc(2.7) = 2` |

#### 4. Utility Library (`<stdlib.h>`)

**Memory Management**

| Function | Description | Notes |
|----------|-------------|-------|
| `malloc()` | Allocate memory | Must check return value |
| `calloc()` | Allocate and zero-initialize memory | Initialized to 0 |
| `realloc()` | Reallocate memory | May move data |
| `free()` | Free memory | Can only free once |

**Program Control**

| Function | Description |
|----------|-------------|
| `exit()` | Normal program exit |
| `abort()` | Abnormal program termination |
| `atexit()` | Register exit function |

**Type Conversion**

| Function | Description | Example |
|----------|-------------|---------|
| `atoi()` | String to integer | `atoi("123") = 123` |
| `atol()` | String to long integer | Same as above |
| `atof()` | String to floating-point | `atof("3.14") = 3.14` |
| `strtol()` | String to long integer | Supports error detection |
| `strtod()` | String to double | Supports error detection |

**Random Numbers**

| Function | Description | Usage |
|----------|-------------|-------|
| `rand()` | Generate random number | `rand() % 100` |
| `srand()` | Set random seed | `srand(time(NULL))` |

**Sorting and Searching**

| Function | Description |
|----------|-------------|
| `qsort()` | Quick sort |
| `bsearch()` | Binary search |

#### 5. Character Classification Library (`<ctype.h>`)

**Character Classification Functions**

| Function | Description | Example |
|----------|-------------|---------|
| `isalnum()` | Letter or digit | `isalnum('A') = true` |
| `isalpha()` | Letter | `isalpha('a') = true` |
| `isdigit()` | Digit | `isdigit('5') = true` |
| `islower()` | Lowercase letter | `islower('z') = true` |
| `isupper()` | Uppercase letter | `isupper('Z') = true` |
| `isspace()` | Whitespace character | `isspace(' ') = true` |
| `isxdigit()` | Hexadecimal digit | `isxdigit('A') = true` |

**Character Conversion Functions**

| Function | Description | Example |
|----------|-------------|---------|
| `tolower()` | Convert to lowercase | `tolower('A') = 'a'` |
| `toupper()` | Convert to uppercase | `toupper('a') = 'A'` |

#### 6. Time Library (`<time.h>`)

**Time Functions**

| Function | Description | Return Value |
|----------|-------------|--------------|
| `time()` | Get current time | Timestamp |
| `clock()` | Get CPU time | Clock ticks |
| `difftime()` | Calculate time difference | Seconds |
| `mktime()` | Convert to timestamp | Timestamp |

**Time Conversion**

| Function | Description | Usage |
|----------|-------------|-------|
| `asctime()` | Time struct to string | Simple format |
| `ctime()` | Timestamp to string | Simple format |
| `gmtime()` | Convert to UTC time | Get UTC |
| `localtime()` | Convert to local time | Get local time |
| `strftime()` | Format time string | Custom format |

## C++ Standard Library

### Standard Headers

#### Input/Output
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<iostream>` | Standard streams | cin, cout, cerr, clog |
| `<fstream>` | File streams | ifstream, ofstream, fstream |
| `<sstream>` | String streams | istringstream, ostringstream |
| `<iomanip>` | Formatting | setw, setprecision |

#### Containers
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<vector>` | Dynamic array | vector |
| `<deque>` | Double-ended queue | deque |
| `<array>` | Fixed-size array | array (C++11) |
| `<list>` | Doubly-linked list | list |
| `<forward_list>` | Singly-linked list | forward_list (C++11) |
| `<stack>` | Stack | stack |
| `<queue>` | Queue | queue, priority_queue |
| `<set>` | Ordered set | set, multiset |
| `<map>` | Ordered map | map, multimap |
| `<unordered_set>` | Unordered set | unordered_set, unordered_multiset (C++11) |
| `<unordered_map>` | Unordered map | unordered_map, unordered_multimap (C++11) |
| `<bitset>` | Bit set | bitset |
| `<valarray>` | Value array | valarray |

#### Algorithms
| Header | Purpose | Key Algorithms |
|--------|---------|----------------|
| `<algorithm>` | General algorithms | sort, find, transform |
| `<numeric>` | Numeric algorithms | accumulate, inner_product |

#### Iterators
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<iterator>` | Iterators | iterator, const_iterator |
| `<memory>` | Memory management | shared_ptr, unique_ptr, allocator |

#### Strings
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<string>` | String class | string, wstring |
| `<string_view>` | String view | string_view (C++17) |
| `<regex>` | Regular expressions | regex, smatch (C++11) |
| `<charconv>` | Character conversion | to_chars, from_chars (C++17) |

#### Function Objects
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<functional>` | Function objects | function, bind, mem_fn |

#### Numerics
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<complex>` | Complex numbers | complex |
| `<valarray>` | Value array | valarray |
| `<numeric>` | Numeric algorithms | accumulate |
| `<ratio>` | Compile-time fractions | ratio (C++11) |
| `<limits>` | Numeric limits | numeric_limits |

#### Date and Time
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<chrono>` | Time library | duration, time_point (C++11) |
| `<ctime>` | C time | time, tm |

#### Localization
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<locale>` | Localization | locale |
| `<codecvt>` | Character encoding conversion | codecvt (C++11, deprecated in C++17) |

#### Exceptions
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<exception>` | Exception class | exception |
| `<stdexcept>` | Standard exceptions | runtime_error, logic_error |
| `<system_error>` | System errors | system_error (C++11) |

#### Utilities
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<utility>` | Utility classes | pair, swap, move, forward |
| `<tuple>` | Tuple | tuple (C++11) |
| `<variant>` | Type-safe union | variant (C++17) |
| `<optional>` | Optional value | optional (C++17) |
| `<any>` | Any type | any (C++17) |
| `<bitset>` | Bit set | bitset |

#### Smart Pointers
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<memory>` | Memory management | unique_ptr, shared_ptr, weak_ptr |

#### Concurrency
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<thread>` | Threads | thread (C++11) |
| `<mutex>` | Mutexes | mutex, lock_guard (C++11) |
| `<condition_variable>` | Condition variables | condition_variable (C++11) |
| `<future>` | Asynchronous operations | promise, future (C++11) |
| `<atomic>` | Atomic operations | atomic (C++11) |

#### Type Traits
| Header | Purpose | Key Functions |
|--------|---------|---------------|
| `<type_traits>` | Type traits | is_same, remove_cv (C++11) |
| `<typeinfo>` | Type information | typeid, type_info |

#### Algorithm Library
| Header | Purpose | Key Algorithms |
|--------|---------|----------------|
| `<algorithm>` | General algorithms | sort, find, transform |
| `<random>` | Random numbers | mt19937, uniform_int_distribution (C++11) |

#### Arrays
| Header | Purpose | Key Types |
|--------|---------|-----------|
| `<array>` | Fixed-size array | array (C++11) |
| `<span>` | Array view | span (C++20) |

### C++ Standard Library Detailed Reference

#### 1. Container Library

**Sequence Containers**

| Container | Features | Time Complexity | Use Cases |
|-----------|----------|-----------------|-----------|
| `vector` | Dynamic array | O(1) access, O(1) back insertion | General-purpose sequence |
| `deque` | Double-ended queue | O(1) insertion at both ends | Operations at both ends |
| `list` | Doubly-linked list | O(1) insertion at any position | Frequent middle insertions |
| `forward_list` | Singly-linked list | O(1) insertion at any position | Memory-efficient |

**Associative Containers**

| Container | Features | Time Complexity | Use Cases |
|-----------|----------|-----------------|-----------|
| `set` | Ordered set | O(log n) | Sorted access needed |
| `map` | Ordered map | O(log n) | Key-value pairs, sorted access needed |
| `multiset` | Allows duplicates | O(log n) | Duplicate keys needed |
| `multimap` | Allows duplicate keys | O(log n) | Duplicate keys needed |
| `unordered_set` | Hash set | Average O(1) | High-performance lookup |
| `unordered_map` | Hash map | Average O(1) | High-performance mapping |

**Container Adapters**

| Adapter | Underlying Container | Features |
|---------|---------------------|----------|
| `stack` | deque/vector | LIFO |
| `queue` | deque/list | FIFO |
| `priority_queue` | vector | Priority queue |

#### 2. Iterators

**Iterator Categories**

| Category | Features | Example |
|----------|----------|---------|
| Input iterator | Read-only, forward | istream_iterator |
| Output iterator | Write-only, forward | ostream_iterator |
| Forward iterator | Read/write, forward | list, set |
| Bidirectional iterator | Read/write, bidirectional | map, set |
| Random access iterator | Random access | vector, array, deque |

**Iterator Operations**

| Operation | Description |
|-----------|-------------|
| `*it` | Dereference |
| `it->member` | Member access |
| `++it, it++` | Advance |
| `--it, it--` | Retreat (bidirectional) |
| `it + n, it - n` | Random access |
| `it1 - it2` | Distance (random access) |
| `it1 < it2` | Comparison (random access) |

#### 3. Algorithm Library

**Non-modifying Algorithms**

| Algorithm | Description | Usage |
|-----------|-------------|-------|
| `find` | Find element | Search |
| `find_if` | Find by condition | Conditional search |
| `count` | Count | Statistics |
| `for_each` | Traverse and process | Execute operation |
| `equal` | Compare equality | Container comparison |
| `mismatch` | Find mismatch | Compare differences |
| `search` | Search subsequence | Sequence matching |

**Modifying Algorithms**

| Algorithm | Description | Usage |
|-----------|-------------|-------|
| `copy` | Copy | Data migration |
| `move` | Move | Efficient migration |
| `transform` | Transform | Data conversion |
| `fill` | Fill | Initialization |
| `generate` | Generate | Populate data |
| `replace` | Replace | Data modification |
| `remove` | Remove | Clean up data |
| `reverse` | Reverse | Reorder |
| `rotate` | Rotate | Circular shift |
| `shuffle` | Shuffle | Randomize |
| `unique` | Deduplicate | Remove duplicates |

**Sorting Algorithms**

| Algorithm | Description | Complexity |
|-----------|-------------|------------|
| `sort` | Sort | Average O(n log n) |
| `stable_sort` | Stable sort | O(n log n) |
| `partial_sort` | Partial sort | O(n log m) |
| `nth_element` | Nth element | Average O(n) |

**Binary Search Algorithms**

| Algorithm | Description | Requirement |
|-----------|-------------|-------------|
| `lower_bound` | First >= | Sorted |
| `upper_bound` | First > | Sorted |
| `binary_search` | Search | Sorted |
| `equal_range` | Range | Sorted |

**Numeric Algorithms**

| Algorithm | Description | Example |
|-----------|-------------|---------|
| `accumulate` | Accumulate | Summation |
| `inner_product` | Inner product | Dot product |
| `partial_sum` | Partial sum | Prefix sum |
| `adjacent_difference` | Adjacent difference | Difference |

#### 4. String Library

**string Class**

| Member Function | Description |
|-----------------|-------------|
| `length()`, `size()` | Length |
| `empty()` | Whether empty |
| `substr()` | Substring |
| `find()`, `rfind()` | Search |
| `replace()` | Replace |
| `insert()` | Insert |
| `erase()` | Erase |
| `append()`, `push_back()` | Append |
| `compare()` | Compare |
| `c_str()` | C string |

#### 5. Smart Pointers

**Smart Pointer Types**

| Type | Features | Usage |
|------|----------|-------|
| `unique_ptr` | Exclusive ownership | Automatic release |
| `shared_ptr` | Shared ownership | Shared across multiple owners |
| `weak_ptr` | Weak reference | Avoid circular references |

**Creation Methods**

| Method | Example |
|--------|---------|
| Direct construction | `unique_ptr<T> ptr(new T)` |
| make_unique | `auto ptr = make_unique<T>()` |
| make_shared | `auto ptr = make_shared<T>()` |

#### 6. Concurrency Library

**Threads**

| Function | Description |
|----------|-------------|
| `thread()` | Create thread |
| `join()` | Wait for completion |
| `detach()` | Detach |
| `hardware_concurrency()` | Thread count |

**Mutexes**

| Type | Features |
|------|----------|
| `mutex` | Basic mutex |
| `recursive_mutex` | Reentrant |
| `timed_mutex` | Timed |
| `shared_mutex` | Read-write lock (C++17) |

**Lock Wrappers**

| Type | Features |
|------|----------|
| `lock_guard` | RAII, simple |
| `unique_lock` | RAII, flexible |
| `scoped_lock` | Multiple locks (C++17) |
| `shared_lock` | Shared lock (C++14) |

## Version Differences

### C Standard Versions

| Version | Year | Major Features |
|---------|------|----------------|
| C89/C90 | 1989/1990 | ANSI/ISO standard |
| C99 | 1999 | bool, long long, variable-length arrays |
| C11 | 2011 | Threads, atomic operations, generic selection |
| C17 | 2018 | Bug fixes for C11 |
| C23 | 2023 | New features (preview) |

### C++ Standard Versions

| Version | Year | Major Features |
|---------|------|----------------|
| C++98 | 1998 | First standard |
| C++03 | 2003 | Bug fix release |
| C++11 | 2011 | Lambda, smart pointers, rvalue references, auto |
| C++14 | 2014 | Generic lambdas, make_unique |
| C++17 | 2017 | Filesystem, string_view, optional |
| C++20 | 2020 | Modules, coroutines, concepts, ranges |
| C++23 | 2023 | New features (preview) |

### Major Version Differences

**Introduced in C++11**

- `auto` type deduction
- Lambda expressions
- Rvalue references and move semantics
- Smart pointers (`unique_ptr`, `shared_ptr`)
- Thread library (`thread`, `mutex`, `condition_variable`)
- Atomic operations (`atomic`)
- Random number library (`<random>`)
- Regular expressions (`<regex>`)
- `tuple`
- `function`, `bind`
- `unordered_map`, `unordered_set`

**Introduced in C++14**

- Generic lambdas
- `make_unique`
- Binary literals (`0b1010`)
- Digit separators (`1'000'000`)
- `std::shared_mutex`

**Introduced in C++17**

- `std::filesystem`
- `std::string_view`
- `std::optional`, `std::variant`, `std::any`
- Structured bindings
- `if constexpr`
- `std::shared_mutex`
- Parallel algorithms
- Fold expressions
- `std::jthread`

**Introduced in C++20**

- Modules (`module`)
- Coroutines (`co_await`, `co_yield`, `co_return`)
- Concepts (`concept`)
- Ranges library (`std::ranges`)
- Three-way comparison operator (`<=>`)
- `std::span`
- Format library (`std::format`)
- Constant evaluation (`consteval`)
- `std::bit`

## Best Practices

### C Language

1. **Memory Management**
   - Always check `malloc` return value
   - Use `malloc`/`free` in pairs
   - Use `valgrind` to detect memory leaks

2. **String Operations**
   - Prefer `strncpy` over `strcpy`
   - Ensure strings are null-terminated with `\0`
   - Avoid buffer overflows

3. **Error Handling**
   - Check all function return values
   - Use `errno` and `perror`
   - Use assertions `assert` appropriately

4. **Portability**
   - Use standard integer types (`<stdint.h>`)
   - Be aware of endianness issues
   - Avoid undefined behavior

### C++ Language

1. **Memory Management**
   - Prefer smart pointers
   - Use `make_shared`, `make_unique`
   - Avoid `new`/`delete`

2. **Container Selection**
   - Prefer `vector`
   - Use `map`/`set` when ordering is needed
   - Use `unordered_map`/`unordered_set` for high performance

3. **Performance Optimization**
   - Use move semantics
   - Prefer pass by reference
   - Use `emplace_back` instead of `push_back`

4. **Modern C++**
   - Use `auto` to simplify code
   - Use lambdas instead of function objects
   - Use range-based for loops

5. **Concurrent Programming**
   - Prefer high-level synchronization primitives
   - Avoid data races
   - Use RAII to manage locks

### General Recommendations

1. **Code Style**
   - Follow naming conventions
   - Add appropriate comments
   - Keep functions concise

2. **Error Handling**
   - Use exceptions (C++)
   - Check return values (C)
   - Provide error messages

3. **Testing**
   - Write unit tests
   - Test boundary conditions
   - Use static analysis tools

4. **Documentation**
   - Use Doxygen
   - Document function parameters and return values
   - Provide usage examples
