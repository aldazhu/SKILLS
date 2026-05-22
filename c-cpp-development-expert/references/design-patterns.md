# C/C++ 设计模式与惯用法

## 目录
- [创建型模式](#创建型模式)
- [结构型模式](#结构型模式)
- [行为型模式](#行为型模式)
- [C++ 惯用法](#c-惯用法)
- [现代 C++ 模式](#现代-c-模式)

## 创建型模式

### 单例模式
```cpp
class Singleton {
private:
    Singleton() = default;
    ~Singleton() = default;
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

public:
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }
    
    void doSomething() {
        // 实现
    }
};

// 使用
Singleton::getInstance().doSomething();
```

### 工厂模式
```cpp
class Product {
public:
    virtual ~Product() = default;
    virtual void operation() = 0;
};

class ConcreteProductA : public Product {
public:
    void operation() override {
        std::cout << "Product A" << std::endl;
    }
};

class ConcreteProductB : public Product {
public:
    void operation() override {
        std::cout << "Product B" << std::endl;
    }
};

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

// 使用
auto product = Factory::createProduct(Factory::ProductType::A);
product->operation();
```

### 构建器模式
```cpp
class Computer {
public:
    class Builder {
    private:
        std::string cpu;
        std::string gpu;
        int ram;
        int storage;
        
    public:
        Builder& setCpu(const std::string& cpu) {
            this->cpu = cpu;
            return *this;
        }
        
        Builder& setGpu(const std::string& gpu) {
            this->gpu = gpu;
            return *this;
        }
        
        Builder& setRam(int ram) {
            this->ram = ram;
            return *this;
        }
        
        Builder& setStorage(int storage) {
            this->storage = storage;
            return *this;
        }
        
        Computer build() {
            return Computer(*this);
        }
        
        friend class Computer;
    };
    
private:
    Computer(const Builder& builder) 
        : cpu(builder.cpu), gpu(builder.gpu), 
          ram(builder.ram), storage(builder.storage) {}
    
    std::string cpu;
    std::string gpu;
    int ram;
    int storage;
    
public:
    void print() {
        std::cout << "CPU: " << cpu << std::endl;
        std::cout << "GPU: " << gpu << std::endl;
        std::cout << "RAM: " << ram << "GB" << std::endl;
        std::cout << "Storage: " << storage << "GB" << std::endl;
    }
};

// 使用
auto computer = Computer::Builder()
    .setCpu("Intel i9")
    .setGpu("NVIDIA RTX 4090")
    .setRam(32)
    .setStorage(2000)
    .build();
computer.print();
```

## 结构型模式

### 适配器模式
```cpp
// 目标接口
class Target {
public:
    virtual ~Target() = default;
    virtual void request() = 0;
};

// 需要适配的类
class Adaptee {
public:
    void specificRequest() {
        std::cout << "Specific request" << std::endl;
    }
};

// 适配器
class Adapter : public Target {
private:
    std::unique_ptr<Adaptee> adaptee;
    
public:
    Adapter() : adaptee(std::make_unique<Adaptee>()) {}
    
    void request() override {
        adaptee->specificRequest();
    }
};

// 使用
std::unique_ptr<Target> target = std::make_unique<Adapter>();
target->request();
```

### 装饰器模式
```cpp
class Component {
public:
    virtual ~Component() = default;
    virtual void operation() = 0;
};

class ConcreteComponent : public Component {
public:
    void operation() override {
        std::cout << "Concrete Component" << std::endl;
    }
};

class Decorator : public Component {
private:
    std::shared_ptr<Component> component;
    
protected:
    Decorator(std::shared_ptr<Component> comp) 
        : component(std::move(comp)) {}
    
    std::shared_ptr<Component> getComponent() const {
        return component;
    }
    
public:
    void operation() override {
        component->operation();
    }
};

class ConcreteDecoratorA : public Decorator {
public:
    ConcreteDecoratorA(std::shared_ptr<Component> comp)
        : Decorator(std::move(comp)) {}
    
    void operation() override {
        std::cout << "Decorator A: ";
        getComponent()->operation();
    }
};

class ConcreteDecoratorB : public Decorator {
public:
    ConcreteDecoratorB(std::shared_ptr<Component> comp)
        : Decorator(std::move(comp)) {}
    
    void operation() override {
        std::cout << "Decorator B: ";
        getComponent()->operation();
    }
};

// 使用
auto component = std::make_shared<ConcreteComponent>();
auto decoratorA = std::make_shared<ConcreteDecoratorA>(component);
auto decoratorB = std::make_shared<ConcreteDecoratorB>(decoratorA);
decoratorB->operation();
```

### 代理模式
```cpp
class Subject {
public:
    virtual ~Subject() = default;
    virtual void request() = 0;
};

class RealSubject : public Subject {
public:
    void request() override {
        std::cout << "Real subject request" << std::endl;
    }
};

class Proxy : public Subject {
private:
    std::unique_ptr<RealSubject> realSubject;
    
public:
    void request() override {
        if (!realSubject) {
            realSubject = std::make_unique<RealSubject>();
        }
        std::cout << "Proxy: Forwarding request" << std::endl;
        realSubject->request();
    }
};

// 使用
std::unique_ptr<Subject> proxy = std::make_unique<Proxy>();
proxy->request();
```

## 行为型模式

### 观察者模式
```cpp
#include <vector>
#include <memory>

class Observer {
public:
    virtual ~Observer() = default;
    virtual void update(const std::string& message) = 0;
};

class Subject {
private:
    std::vector<std::weak_ptr<Observer>> observers;
    
public:
    void attach(const std::shared_ptr<Observer>& observer) {
        observers.push_back(observer);
    }
    
    void notify(const std::string& message) {
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

class ConcreteObserver : public Observer {
private:
    std::string name;
    
public:
    ConcreteObserver(const std::string& name) : name(name) {}
    
    void update(const std::string& message) override {
        std::cout << name << " received: " << message << std::endl;
    }
};

// 使用
Subject subject;
auto observer1 = std::make_shared<ConcreteObserver>("Observer 1");
auto observer2 = std::make_shared<ConcreteObserver>("Observer 2");

subject.attach(observer1);
subject.attach(observer2);
subject.notify("Hello, observers!");
```

### 策略模式
```cpp
class Strategy {
public:
    virtual ~Strategy() = default;
    virtual int execute(int a, int b) = 0;
};

class AddStrategy : public Strategy {
public:
    int execute(int a, int b) override {
        return a + b;
    }
};

class MultiplyStrategy : public Strategy {
public:
    int execute(int a, int b) override {
        return a * b;
    }
};

class Context {
private:
    std::unique_ptr<Strategy> strategy;
    
public:
    void setStrategy(std::unique_ptr<Strategy> s) {
        strategy = std::move(s);
    }
    
    int executeStrategy(int a, int b) {
        return strategy->execute(a, b);
    }
};

// 使用
Context context;
context.setStrategy(std::make_unique<AddStrategy>());
std::cout << "Add: " << context.executeStrategy(5, 3) << std::endl;

context.setStrategy(std::make_unique<MultiplyStrategy>());
std::cout << "Multiply: " << context.executeStrategy(5, 3) << std::endl;
```

### 命令模式
```cpp
class Command {
public:
    virtual ~Command() = default;
    virtual void execute() = 0;
};

class Receiver {
public:
    void action() {
        std::cout << "Receiver action" << std::endl;
    }
};

class ConcreteCommand : public Command {
private:
    std::shared_ptr<Receiver> receiver;
    
public:
    ConcreteCommand(std::shared_ptr<Receiver> r) 
        : receiver(std::move(r)) {}
    
    void execute() override {
        receiver->action();
    }
};

class Invoker {
private:
    std::vector<std::unique_ptr<Command>> commands;
    
public:
    void addCommand(std::unique_ptr<Command> cmd) {
        commands.push_back(std::move(cmd));
    }
    
    void executeCommands() {
        for (auto& cmd : commands) {
            cmd->execute();
        }
        commands.clear();
    }
};

// 使用
auto receiver = std::make_shared<Receiver>();
auto command = std::make_unique<ConcreteCommand>(receiver);
Invoker invoker;
invoker.addCommand(std::move(command));
invoker.executeCommands();
```

## C++ 惯用法

### RAII (Resource Acquisition Is Initialization)
```cpp
class FileHandler {
private:
    FILE* file;
    
public:
    explicit FileHandler(const char* filename, const char* mode) {
        file = fopen(filename, mode);
        if (!file) {
            throw std::runtime_error("Cannot open file");
        }
    }
    
    ~FileHandler() {
        if (file) {
            fclose(file);
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
};

// 使用
{
    FileHandler file("data.txt", "r");
    // 使用文件
    // 离开作用域时自动关闭
}
```

### Pimpl 惯用法
```cpp
// MyClass.h
class MyClass {
private:
    class Impl;
    std::unique_ptr<Impl> impl;
    
public:
    MyClass();
    ~MyClass();
    
    void doSomething();
    int getValue() const;
};

// MyClass.cpp
#include "MyClass.h"

class MyClass::Impl {
public:
    int value = 42;
    
    void doSomething() {
        std::cout << "Implementation: " << value << std::endl;
    }
};

MyClass::MyClass() : impl(std::make_unique<Impl>()) {}

MyClass::~MyClass() = default; // 需要 Impl 类完整定义

void MyClass::doSomething() {
    impl->doSomething();
}

int MyClass::getValue() const {
    return impl->value;
}
```

### CRTP (Curiously Recurring Template Pattern)
```cpp
template <typename Derived>
class Base {
public:
    void interface() {
        static_cast<Derived*>(this)->implementation();
    }
    
    void common() {
        std::cout << "Common functionality" << std::endl;
    }
};

class Derived : public Base<Derived> {
public:
    void implementation() {
        std::cout << "Derived implementation" << std::endl;
        common();
    }
};

// 使用
Derived d;
d.interface();
```

### 类型擦除
```cpp
class Any {
private:
    struct Concept {
        virtual ~Concept() = default;
        virtual void* get() = 0;
        virtual std::unique_ptr<Concept> clone() const = 0;
    };
    
    template <typename T>
    struct Model : Concept {
        T data;
        
        Model(const T& value) : data(value) {}
        Model(T&& value) : data(std::move(value)) {}
        
        void* get() override {
            return &data;
        }
        
        std::unique_ptr<Concept> clone() const override {
            return std::make_unique<Model<T>>(data);
        }
    };
    
    std::unique_ptr<Concept> object;
    
public:
    Any() = default;
    
    template <typename T>
    Any(T&& value) 
        : object(std::make_unique<Model<std::decay_t<T>>>(
              std::forward<T>(value))) {}
    
    Any(const Any& other) 
        : object(other.object ? other.object->clone() : nullptr) {}
    
    Any(Any&&) noexcept = default;
    Any& operator=(Any&&) noexcept = default;
    
    template <typename T>
    T& cast() {
        return *static_cast<T*>(object->get());
    }
};

// 使用
Any a = 42;
int value = a.cast<int>();
std::cout << value << std::endl; // 42
```

## 现代 C++ 模式

### 基于范围的工厂 (C++20)
```cpp
#include <memory>
#include <concepts>

template <typename T>
concept Factory = requires(T factory) {
    { factory() } -> std::same_as<std::unique_ptr<int>>;
};

std::unique_ptr<int> create_int(int value) {
    return std::make_unique<int>(value);
}

// 使用
auto factory = [] { return std::make_unique<int>(42); };
static_assert(Factory<decltype(factory)>);
auto ptr = factory();
```

### 类型安全的联合 (C++17)
```cpp
#include <variant>
#include <stdexcept>

using Value = std::variant<int, double, std::string>;

void process_value(const Value& value) {
    std::visit([](auto&& arg) {
        using T = std::decay_t<decltype(arg)>;
        if constexpr (std::is_same_v<T, int>) {
            std::cout << "Int: " << arg << std::endl;
        } else if constexpr (std::is_same_v<T, double>) {
            std::cout << "Double: " << arg << std::endl;
        } else if constexpr (std::is_same_v<T, std::string>) {
            std::cout << "String: " << arg << std::endl;
        }
    }, value);
}

// 使用
Value v1 = 42;
Value v2 = 3.14;
Value v3 = "hello";

process_value(v1);
process_value(v2);
process_value(v3);
```

### 期望值模式 (C++23)
```cpp
#include <expected>
#include <string>

std::expected<int, std::string> divide(int a, int b) {
    if (b == 0) {
        return std::unexpected("Division by zero");
    }
    return a / b;
}

// 使用
auto result = divide(10, 2);
if (result) {
    std::cout << "Result: " << *result << std::endl;
} else {
    std::cout << "Error: " << result.error() << std::endl;
}
```

### 协程模式 (C++20)
```cpp
#include <coroutine>
#include <iostream>

struct Task {
    struct promise_type {
        Task get_return_object() {
            return Task{std::coroutine_handle<promise_type>::from_promise(*this)};
        }
        std::suspend_never initial_suspend() { return {}; }
        std::suspend_never final_suspend() noexcept { return {}; }
        void return_void() {}
        void unhandled_exception() { std::terminate(); }
    };

    std::coroutine_handle<promise_type> h;
    Task(std::coroutine_handle<promise_type> handle) : h(handle) {}
    ~Task() { if (h) h.destroy(); }
};

Task async_function() {
    std::cout << "Coroutine started" << std::endl;
    co_await std::suspend_always{};
    std::cout << "Coroutine resumed" << std::endl;
    co_return;
}

// 使用
int main() {
    auto task = async_function();
    return 0;
}
```

### 策略模式的现代实现 (C++20)
```cpp
#include <concepts>
#include <functional>

template <typename T>
concept Strategy = requires(T s, int a, int b) {
    { s(a, b) } -> std::same_as<int>;
};

void execute_strategy(Strategy auto&& strategy, int a, int b) {
    int result = strategy(a, b);
    std::cout << "Result: " << result << std::endl;
}

// 使用
int main() {
    auto add = [](int a, int b) { return a + b; };
    auto multiply = [](int a, int b) { return a * b; };
    
    execute_strategy(add, 5, 3);      // 8
    execute_strategy(multiply, 5, 3); // 15
    return 0;
}
```

## 性能优化模式

### 对象池模式
```cpp
#include <stack>
#include <memory>

template <typename T>
class ObjectPool {
private:
    std::stack<std::unique_ptr<T>> pool;
    
public:
    ObjectPool(size_t initialSize = 0) {
        for (size_t i = 0; i < initialSize; ++i) {
            pool.push(std::make_unique<T>());
        }
    }
    
    std::unique_ptr<T> acquire() {
        if (pool.empty()) {
            return std::make_unique<T>();
        }
        auto obj = std::move(pool.top());
        pool.pop();
        return obj;
    }
    
    void release(std::unique_ptr<T> obj) {
        pool.push(std::move(obj));
    }
};

// 使用
ObjectPool<MyClass> pool(10);
auto obj = pool.acquire();
// 使用对象
pool.release(std::move(obj));
```

### 延迟初始化
```cpp
#include <optional>
#include <mutex>

class LazyInitializer {
private:
    mutable std::mutex mtx;
    mutable std::optional<int> value;
    
public:
    int getValue() const {
        std::lock_guard<std::mutex> lock(mtx);
        if (!value) {
            value = expensiveComputation();
        }
        return *value;
    }
    
private:
    int expensiveComputation() const {
        // 耗时计算
        return 42;
    }
};

// C++20 方式
#include <mutex>
#include <optional>

class LazyInitializerCXX20 {
private:
    mutable std::mutex mtx;
    mutable std::once_flag flag;
    mutable std::optional<int> value;
    
public:
    int getValue() const {
        std::call_once(flag, [this] {
            value = expensiveComputation();
        });
        return *value;
    }
    
private:
    int expensiveComputation() const {
        return 42;
    }
};
```

## 线程安全模式

### 双重检查锁定
```cpp
class Singleton {
private:
    static std::unique_ptr<Singleton> instance;
    static std::mutex mtx;
    
    Singleton() = default;
    
public:
    static Singleton* getInstance() {
        if (!instance) {
            std::lock_guard<std::mutex> lock(mtx);
            if (!instance) {
                instance = std::make_unique<Singleton>();
            }
        }
        return instance.get();
    }
};

// C++11+ 更简单的方式
class SingletonCXX11 {
public:
    static SingletonCXX11& getInstance() {
        static SingletonCXX11 instance;
        return instance;
    }
    
private:
    SingletonCXX11() = default;
    ~SingletonCXX11() = default;
    SingletonCXX11(const SingletonCXX11&) = delete;
    SingletonCXX11& operator=(const SingletonCXX11&) = delete;
};
```

### 读写锁
```cpp
#include <shared_mutex>

class ThreadSafeCache {
private:
    mutable std::shared_mutex mtx;
    std::unordered_map<std::string, int> cache;
    
public:
    void set(const std::string& key, int value) {
        std::unique_lock<std::shared_mutex> lock(mtx);
        cache[key] = value;
    }
    
    int get(const std::string& key) const {
        std::shared_lock<std::shared_mutex> lock(mtx);
        auto it = cache.find(key);
        if (it != cache.end()) {
            return it->second;
        }
        return -1; // Not found
    }
};
```

## 最佳实践总结

1. **优先使用现代 C++ 特性**：智能指针、RAII、移动语义
2. **避免过度设计**：选择最简单有效的解决方案
3. **考虑性能影响**：虚拟函数、动态分配的开销
4. **保持接口简洁**：清晰易用的 API 设计
5. **使用组合优于继承**：更灵活的代码结构
6. **遵循 SOLID 原则**：单一职责、开闭原则等
7. **注意线程安全**：多线程环境下的正确性
8. **资源管理自动化**：使用 RAII 避免资源泄漏
9. **错误处理清晰**：异常 vs 错误码的选择
10. **代码可读性优先**：清晰的表达意图
