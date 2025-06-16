# C++ 学习笔记

## 一、基础语法
基础语法是学习 C++ 的基石，它包括变量、数据类型、控制结构等基本元素。

### 知识点
1. **数据类型**
    - **基本数据类型**：如 `int`（整数类型）、`float`（单精度浮点类型）、`double`（双精度浮点类型）、`char`（字符类型）、`bool`（布尔类型）等。
    - **复合数据类型**：数组、结构体、枚举等。例如，数组可以存储相同类型的多个元素，结构体可以将不同类型的数据组合在一起。
2. **变量声明与初始化**
    - 声明变量时需要指定数据类型，如 `int num;` 声明了一个整数变量 `num`。
    - 初始化可以在声明时进行，如 `int num = 10;`，也可以使用构造函数初始化，如 `std::string str("Hello");`。
3. **控制结构**
    - **条件语句**：`if-else` 用于根据条件执行不同的代码块，`switch` 用于多分支选择。
    - **循环语句**：`for` 循环适用于已知循环次数的情况，`while` 循环和 `do-while` 循环适用于未知循环次数的情况。
4. **函数**
    - 函数的定义包括返回类型、函数名、参数列表和函数体。例如：
```cpp
int add(int a, int b) {
    return a + b;
}
```
    - 函数调用时需要传递相应的参数，如 `int result = add(3, 5);`。
    - 函数还支持默认参数、函数重载等特性。

### 案例
```cpp
#include <iostream>
#include <string>

// 结构体定义
struct Person {
    std::string name;
    int age;
};

// 函数定义，使用默认参数
void printPerson(const Person& p, bool printAge = true) {
    std::cout << "Name: " << p.name;
    if (printAge) {
        std::cout << ", Age: " << p.age;
    }
    std::cout << std::endl;
}

int main() {
    // 变量声明与初始化
    int x = 5;
    float y = 3.14f;
    char c = 'A';
    bool isTrue = true;

    // 数组初始化
    int arr[3] = {1, 2, 3};

    // 结构体初始化
    Person person = {"John", 25};

    // 函数调用
    int result = add(x, static_cast<int>(y));
    std::cout << "Add result: " << result << std::endl;

    printPerson(person);
    printPerson(person, false);

    // 控制结构示例
    if (isTrue) {
        std::cout << "It's true!" << std::endl;
    }

    for (int i = 0; i < 3; ++i) {
        std::cout << "Array element: " << arr[i] << std::endl;
    }

    return 0;
}
```

## 二、异常处理
异常处理机制允许程序在运行时处理错误情况，避免程序崩溃。

### 知识点
1. **`try-catch` 块**
    - `try` 块中包含可能抛出异常的代码，`catch` 块用于捕获和处理异常。可以有多个 `catch` 块，分别处理不同类型的异常。
2. **`throw` 语句**
    - 用于抛出异常，可以抛出内置类型或自定义异常类型。例如：`throw std::runtime_error("Something went wrong!");`
3. **标准异常类**
    - C++ 标准库提供了一系列异常类，如 `std::exception` 是所有标准异常类的基类，`std::runtime_error` 用于表示运行时错误，`std::logic_error` 用于表示逻辑错误等。
4. **自定义异常类**
    - 可以通过继承 `std::exception` 类来创建自定义异常类，并重写 `what()` 方法。

### 案例
```cpp
#include <iostream>
#include <stdexcept>

// 自定义异常类
class DivideByZeroException : public std::exception {
public:
    const char* what() const noexcept override {
        return "Division by zero!";
    }
};

double divide(int a, int b) {
    if (b == 0) {
        // 抛出自定义异常
        throw DivideByZeroException();
    }
    return static_cast<double>(a) / b;
}

int main() {
    try {
        int x = 10;
        int y = 0;
        double result = divide(x, y);
        std::cout << "The result is: " << result << std::endl;
    } catch (const DivideByZeroException& e) {
        // 捕获自定义异常
        std::cerr << "Exception caught: " << e.what() << std::endl;
    } catch (const std::exception& e) {
        // 捕获其他标准异常
        std::cerr << "Generic exception caught: " << e.what() << std::endl;
    }
    return 0;
}
```

## 三、线程管理
C++11 引入了标准线程库，允许程序创建和管理多个线程。

### 知识点
1. **`std::thread` 类**
    - 用于创建和管理线程，可以通过构造函数传递可调用对象（函数、函数对象、Lambda 表达式等）和参数来创建线程。
2. **线程的创建和启动**
    - 例如：`std::thread t(func, args...);`，线程创建后会立即启动执行。
3. **线程的等待和分离**
    - `t.join()` 会阻塞当前线程，直到被等待的线程执行结束。
    - `t.detach()` 会使线程在后台独立运行，当前线程将失去对该线程的直接控制权。
4. **线程生命周期管理**
    - 需要确保对线程对象调用 `join` 或 `detach` 方法，否则在线程对象析构时会触发 `std::terminate` 函数，导致程序异常终止。可以使用 RAII 技术来管理线程资源。

### 案例
```cpp
#include <iostream>
#include <thread>
#include <stdexcept>

// RAII 类用于管理线程资源
class ThreadGuard {
public:
    explicit ThreadGuard(std::thread& t) : t_(t) {}
    ~ThreadGuard() {
        if (t_.joinable()) {
            t_.join();
        }
    }
    ThreadGuard(const ThreadGuard&) = delete;
    ThreadGuard& operator=(const ThreadGuard&) = delete;
private:
    std::thread& t_;
};

void hello() {
    std::cout << "Hello from thread!" << std::endl;
}

void throwException() {
    throw std::runtime_error("Exception in thread!");
}

int main() {
    // 创建并启动线程
    std::thread t(hello);
    ThreadGuard guard(t);

    try {
        std::thread t2(throwException);
        ThreadGuard guard2(t2);
    } catch (const std::exception& e) {
        std::cerr << "Exception caught: " << e.what() << std::endl;
    }

    std::cout << "Hello from main!" << std::endl;
    return 0;
}
```

## 四、同步机制
同步机制用于协调多个线程的执行，避免数据竞争和不一致性。

### 知识点
1. **互斥锁（`std::mutex`）**
    - 用于保护共享资源，确保同一时间只有一个线程可以访问。`std::mutex` 是最基本的互斥量，`std::recursive_mutex` 允许同一个线程多次获取该锁。
2. **锁的获取和释放**
    - `std::lock_guard` 构造时自动加锁，析构时自动解锁，不支持手动释放锁。
    - `std::unique_lock` 比 `std::lock_guard` 更灵活，支持延迟加锁、手动解锁，还可以与条件变量配合使用。
    - `std::scoped_lock`（C++17）支持一次性获取多个锁，避免死锁问题。
3. **条件变量（`std::condition_variable`）**
    - 用于线程间的事件通知，通常与 `std::unique_lock` 配合使用。需要使用谓词循环检查来避免虚假唤醒。
4. **避免死锁**
    - 可以通过固定锁获取顺序、使用 `std::lock` 或 `std::scoped_lock`、避免持锁时调用用户代码等方法来避免死锁。

### 案例
```cpp
#include <iostream>
#include <thread>
#include <mutex>
#include <condition_variable>

std::mutex mtx;
std::condition_variable cv;
int shared_variable = 0;
bool ready = false;

void increment() {
    for (int i = 0; i < 100000; ++i) {
        // 自动获取和释放锁
        std::lock_guard<std::mutex> lock(mtx);
        ++shared_variable;
    }

    {
        std::lock_guard<std::mutex> lock(mtx);
        ready = true;
    }
    cv.notify_one();
}

void waitForReady() {
    std::unique_lock<std::mutex> lk(mtx);
    cv.wait(lk, []{ return ready; });
    std::cout << "Shared variable: " << shared_variable << std::endl;
}

int main() {
    std::thread t1(increment);
    std::thread t2(waitForReady);

    t1.join();
    t2.join();

    return 0;
}
```

## 五、高级工具
高级工具提供了更强大和灵活的编程能力，如智能指针和函数对象。

### 知识点
1. **智能指针**
    - **`std::unique_ptr`**：独占所有权的智能指针，同一时间只能有一个 `std::unique_ptr` 指向同一个对象。
    - **`std::shared_ptr`**：共享所有权的智能指针，多个 `std::shared_ptr` 可以指向同一个对象，通过引用计数来管理对象的生命周期。
    - **`std::weak_ptr`**：弱引用智能指针，用于解决 `std::shared_ptr` 的循环引用问题，它不增加引用计数。
2. **函数对象（`std::function`）**
    - 可以存储、复制和调用任何可调用对象，如函数、函数指针、函数对象、Lambda 表达式等。
3. **lambda 表达式**
    - 一种匿名函数，用于创建简洁的函数对象。可以捕获外部变量，分为值捕获和引用捕获。

### 案例
```cpp
#include <iostream>
#include <memory>
#include <functional>

// 函数对象类
class Adder {
public:
    int operator()(int a, int b) const {
        return a + b;
    }
};

int main() {
    // 智能指针
    std::unique_ptr<int> uniquePtr = std::make_unique<int>(42);
    std::cout << "Unique pointer value: " << *uniquePtr << std::endl;

    std::shared_ptr<int> sharedPtr1 = std::make_shared<int>(10);
    std::shared_ptr<int> sharedPtr2 = sharedPtr1;
    std::cout << "Shared pointer use count: " << sharedPtr1.use_count() << std::endl;

    std::weak_ptr<int> weakPtr = sharedPtr1;
    if (auto lockedPtr = weakPtr.lock()) {
        std::cout << "Weak pointer value: " << *lockedPtr << std::endl;
    }

    // 函数对象
    Adder adder;
    std::function<int(int, int)> func = adder;
    std::cout << "Function object result: " << func(3, 5) << std::endl;

    // lambda 表达式
    auto lambda = [](int x) { return x * 2; };
    std::cout << "Lambda result: " << lambda(5) << std::endl;

    int value = 10;
    auto captureLambda = [value](int x) { return x + value; };
    std::cout << "Captured lambda result: " << captureLambda(3) << std::endl;

    return 0;
}
```

## 六、原子操作
原子操作是不可分割的操作，用于在多线程环境中实现无锁编程。

### 知识点
1. **`std::atomic` 模板类**
    - 用于创建原子类型，如 `std::atomic<int>`、`std::atomic<bool>` 等。
2. **原子操作**
    - 如 `load()` 用于读取原子变量的值，`store()` 用于写入原子变量的值，`fetch_add()` 用于原子地增加变量的值等。
3. **内存顺序（Memory Order）**
    - 包括 `std::memory_order_relaxed`（仅保证原子性，没有顺序约束）、`std::memory_order_acquire`（读屏障）、`std::memory_order_release`（写屏障）、`std::memory_order_seq_cst`（顺序一致性，默认内存序）等。

### 案例
```cpp
#include <iostream>
#include <thread>
#include <atomic>

std::atomic<int> atomic_variable(0);

void atomic_increment() {
    for (int i = 0; i < 100000; ++i) {
        atomic_variable.fetch_add(1, std::memory_order_relaxed);
    }
}

int main() {
    std::thread t1(atomic_increment);
    std::thread t2(atomic_increment);

    t1.join();
    t2.join();

    std::cout << "Atomic variable: " << atomic_variable << std::endl;
    return 0;
}
```

## 七、并发结构
并发结构是为多线程环境设计的数据结构，提供高效的并发访问。

### 知识点
1. **并发队列（`std::queue` + 同步机制）**
    - 用于线程间的数据传递，需要使用互斥锁和条件变量来保证线程安全。
2. **并发哈希表**
    - 提供高效的并发查找和插入操作，可以使用细粒度锁或无锁算法来实现。
3. **基于锁的并发数据结构设计**
    - 如线程安全的栈、队列等，需要使用互斥锁来保护共享数据。
4. **无锁设计**
    - 如使用 CAS（Compare-And-Swap）操作实现无锁数据结构，避免锁带来的开销。

### 案例
```cpp
#include <iostream>
#include <thread>
#include <queue>
#include <mutex>
#include <condition_variable>

template <typename T>
class ConcurrentQueue {
private:
    std::queue<T> queue_;
    mutable std::mutex mtx_;
    std::condition_variable cond_var_;

public:
    void push(T value) {
        std::lock_guard<std::mutex> lock(mtx_);
        queue_.push(std::move(value));
        cond_var_.notify_one();
    }

    T pop() {
        std::unique_lock<std::mutex> lock(mtx_);
        cond_var_.wait(lock, [this] { return !queue_.empty(); });
        T value = std::move(queue_.front());
        queue_.pop();
        return value;
    }
};

ConcurrentQueue<int> queue;

void producer() {
    for (int i = 0; i < 10; ++i) {
        queue.push(i);
        std::this_thread::sleep_for(std::chrono::milliseconds(100));
    }
}

void consumer() {
    for (int i = 0; i < 10; ++i) {
        int value = queue.pop();
        std::cout << "Consumed: " << value << std::endl;
    }
}

int main() {
    std::thread t1(producer);
    std::thread t2(consumer);

    t1.join();
    t2.join();

    return 0;
}
```

## 八、实战优化
实战优化涉及到如何在实际项目中应用上述知识，提高程序的性能和可靠性。

### 知识点
1. **性能分析**
    - 使用工具如 `gprof`、`valgrind` 等分析程序的性能瓶颈，找出耗时的函数和内存泄漏问题。
2. **并发算法优化**
    - 如并行排序、并行搜索等，可以使用 C++17 引入的并行算法库来实现。
3. **内存管理优化**
    - 合理使用智能指针和内存池，避免频繁的内存分配和释放带来的开销。
4. **线程池与工作窃取**
    - 线程池可以避免频繁创建和销毁线程的开销，提高性能。工作窃取算法可以提高线程的利用率。

### 案例
```cpp
#include <iostream>
#include <vector>
#include <thread>
#include <numeric>
#include <algorithm>
#include <execution>

// 并行求和函数
int parallel_sum(const std::vector<int>& data) {
    return std::reduce(std::execution::par, data.begin(), data.end());
}

int main() {
    std::vector<int> data(1000000);
    std::iota(data.begin(), data.end(), 1);

    int result = parallel_sum(data);
    std::cout << "Sum: " << result << std::endl;

    return 0;
}
```
