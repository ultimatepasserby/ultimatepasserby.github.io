### 一、Java基础 (Core Java)

#### 1. 语法基础
- **数据类型**：Java有基本数据类型（如`int`, `double`, `char`等）和引用数据类型（如类、接口、数组）。
- **变量**：用于存储数据的容器，使用前需声明并赋值。
- **运算符**：如算术运算符（`+`, `-`, `*`, `/`）、逻辑运算符（`&&`, `||`, `!`）等。
- **流程控制**：包括`if/else`, `switch`, `for`, `while`, `do-while`等语句，用于控制程序的执行流程。
- **数组**：用于存储相同类型数据的有序集合。

```java
public class SyntaxBasics {
    public static void main(String[] args) {
        // 数据类型和变量
        int num = 10;
        double pi = 3.14;
        char ch = 'A';

        // 运算符
        int result = num + 5;

        // 流程控制
        if (result > 15) {
            System.out.println("Result is greater than 15");
        } else {
            System.out.println("Result is less than or equal to 15");
        }

        // 数组
        int[] arr = new int[5];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i * 2;
        }
        for (int value : arr) {
            System.out.print(value + " ");
        }
    }
}
```

#### 2. 面向对象编程(OOP)
- **核心概念**
    - **类与对象**：类是对象的抽象，对象是类的实例。
    - **封装**：将数据和操作数据的方法绑定在一起，隐藏对象的内部实现细节。
    - **继承**：子类继承父类的属性和方法，实现代码复用。
    - **多态**：同一个方法可以根据对象的不同类型表现出不同的行为。
    - **抽象**：抽象类和接口用于定义抽象方法，强制子类实现。

```java
// 抽象类
abstract class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public abstract void makeSound();
}

// 子类
class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }

    @Override
    public void makeSound() {
        System.out.println(name + " says Woof!");
    }
}

public class OOPExample {
    public static void main(String[] args) {
        Animal dog = new Dog("Buddy");
        dog.makeSound();
    }
}
```

#### 3. 常用类库
- **`Object`类**：所有类的父类，提供了`toString()`, `equals()`, `hashCode()`等方法。
- **字符串**：`String`是不可变的，`StringBuilder`和`StringBuffer`是可变的，`StringBuffer`是线程安全的。
- **包装类**：将基本数据类型封装成对象，支持自动装箱和拆箱。
- **日期时间API**：Java 8引入的`java.time`包提供了更方便的日期时间处理类。
- **异常处理**：使用`try-catch-finally`块捕获和处理异常，也可以使用`try-with-resources`自动关闭资源。
- **泛型**：提供类型安全，消除强制类型转换。
- **反射**：在运行时获取类的信息，动态创建对象、调用方法和访问字段。
- **注解**：为代码提供元数据，可用于编译时检查、运行时处理等。
- **I/O流**：用于读写数据，分为字节流和字符流。

```java
import java.io.FileReader;
import java.io.IOException;
import java.lang.reflect.Method;
import java.time.LocalDate;
import java.util.ArrayList;
import java.util.List;

// 注解
@interface MyAnnotation {
    String value();
}

// 使用注解
@MyAnnotation("Test Annotation")
class MyClass {
    public void printMessage() {
        System.out.println("Hello, World!");
    }
}

public class CommonLibraryExample {
    public static void main(String[] args) {
        // Object类
        Object obj = new Object();
        System.out.println(obj.toString());

        // 字符串
        String str = "Hello";
        StringBuilder sb = new StringBuilder(str);
        sb.append(" World");
        System.out.println(sb.toString());

        // 包装类
        Integer num = 10; // 自动装箱
        int n = num; // 自动拆箱

        // 日期时间API
        LocalDate today = LocalDate.now();
        System.out.println("Today: " + today);

        // 异常处理
        try (FileReader fr = new FileReader("test.txt")) {
            int data;
            while ((data = fr.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 泛型
        List<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        for (String item : list) {
            System.out.println(item);
        }

        // 反射
        try {
            Class<?> clazz = MyClass.class;
            Object instance = clazz.getDeclaredConstructor().newInstance();
            Method method = clazz.getMethod("printMessage");
            method.invoke(instance);
        } catch (Exception e) {
            e.printStackTrace();
        }

        // 注解
        MyClass myClass = new MyClass();
        MyAnnotation annotation = myClass.getClass().getAnnotation(MyAnnotation.class);
        if (annotation != null) {
            System.out.println("Annotation value: " + annotation.value());
        }
    }
}
```

### 二、Java集合框架 (Java Collections Framework)

#### 1. 核心接口
- `Collection`：是所有集合的根接口，定义了集合的基本操作。
- `List`：有序集合，允许重复元素。
- `Set`：无序集合，不允许重复元素。
- `Queue`/`Deque`：队列和双端队列，用于实现先进先出（FIFO）或后进先出（LIFO）的操作。
- `Map`：键值对的集合，键是唯一的。

#### 2. 常用实现类
- **`List`**：`ArrayList`基于数组实现，随机访问快；`LinkedList`基于双向链表实现，插入删除快。
- **`Set`**：`HashSet`基于`HashMap`实现，无序；`TreeSet`基于红黑树实现，有序。
- **`Queue`/`Deque`**：`LinkedList`可作为队列和双端队列使用；`PriorityQueue`是优先队列。
- **`Map`**：`HashMap`基于数组+链表/红黑树实现，重点掌握其原理、哈希冲突和扩容；`ConcurrentHashMap`是线程安全的。

```java
import java.util.*;

public class CollectionExample {
    public static void main(String[] args) {
        // List
        List<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");
        System.out.println("List: " + list);

        // Set
        Set<String> set = new HashSet<>();
        set.add("Apple");
        set.add("Banana");
        set.add("Apple"); // 重复元素不会被添加
        System.out.println("Set: " + set);

        // Queue
        Queue<String> queue = new LinkedList<>();
        queue.add("Apple");
        queue.add("Banana");
        queue.add("Cherry");
        System.out.println("Queue: " + queue);
        System.out.println("Poll: " + queue.poll());

        // Map
        Map<String, Integer> map = new HashMap<>();
        map.put("Apple", 1);
        map.put("Banana", 2);
        map.put("Cherry", 3);
        System.out.println("Map: " + map);
    }
}
```

#### 3. 工具类
- `Collections`：提供了排序、查找、同步包装等方法。
- `Arrays`：提供了操作数组的方法，如排序、查找等。

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class CollectionUtilsExample {
    public static void main(String[] args) {
        // Collections工具类
        List<Integer> list = Arrays.asList(3, 1, 2);
        Collections.sort(list);
        System.out.println("Sorted list: " + list);

        // Arrays工具类
        int[] arr = {3, 1, 2};
        Arrays.sort(arr);
        System.out.println("Sorted array: " + Arrays.toString(arr));
    }
}
```

#### 4. 关键知识点
- **迭代器**：用于遍历集合元素。
- **`fail-fast`机制与`fail-safe`机制**：`fail-fast`在集合结构发生变化时会抛出`ConcurrentModificationException`；`fail-safe`则不会。
- **`Comparable`与`Comparator`接口**：用于定义对象的排序规则。

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Iterator;
import java.util.List;

class Person implements Comparable<Person> {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int compareTo(Person other) {
        return this.age - other.age;
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }
}

public class CollectionKeyPointsExample {
    public static void main(String[] args) {
        // 迭代器
        List<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");
        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

        // Comparable接口
        List<Person> people = new ArrayList<>();
        people.add(new Person("Alice", 25));
        people.add(new Person("Bob", 20));
        people.add(new Person("Charlie", 30));
        Collections.sort(people);
        System.out.println("Sorted people: " + people);
    }
}
```

### 三、Java并发编程 (Java Concurrency/Multithreading)

#### 1. 核心概念
- **进程 vs 线程**：进程是程序在操作系统中的一次执行过程，线程是进程中的一个执行单元。
- **线程生命周期/状态**：包括`NEW`, `RUNNABLE`, `BLOCKED`, `WAITING`, `TIMED_WAITING`, `TERMINATED`。
- **上下文切换**：操作系统在多个线程之间切换执行上下文。

#### 2. 线程创建与使用
- **继承`Thread`类**：重写`run()`方法。
- **实现`Runnable`接口**：实现`run()`方法。
- **实现`Callable`接口 + `Future`/`FutureTask`**：可返回结果。

```java
import java.util.concurrent.*;

// 继承Thread类
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running: " + Thread.currentThread().getName());
    }
}

// 实现Runnable接口
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Runnable is running: " + Thread.currentThread().getName());
    }
}

// 实现Callable接口
class MyCallable implements Callable<String> {
    @Override
    public String call() throws Exception {
        return "Callable result";
    }
}

public class ThreadCreationExample {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 继承Thread类
        MyThread thread = new MyThread();
        thread.start();

        // 实现Runnable接口
        MyRunnable runnable = new MyRunnable();
        Thread runnableThread = new Thread(runnable);
        runnableThread.start();

        // 实现Callable接口
        MyCallable callable = new MyCallable();
        ExecutorService executor = Executors.newSingleThreadExecutor();
        Future<String> future = executor.submit(callable);
        System.out.println("Callable result: " + future.get());
        executor.shutdown();
    }
}
```

#### 3. 线程池
- **核心原理**：`ThreadPoolExecutor`及其核心参数（核心线程数、最大线程数、工作队列、拒绝策略、线程工厂、存活时间）。
- **`Executors`工厂类**：常用但不推荐在生产环境直接使用，推荐手动创建`ThreadPoolExecutor`。

```java
import java.util.concurrent.*;

public class ThreadPoolExample {
    public static void main(String[] args) {
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                2, // 核心线程数
                5, // 最大线程数
                60, // 线程存活时间
                TimeUnit.SECONDS,
                new LinkedBlockingQueue<>(10), // 工作队列
                Executors.defaultThreadFactory(), // 线程工厂
                new ThreadPoolExecutor.AbortPolicy() // 拒绝策略
        );

        for (int i = 0; i < 10; i++) {
            final int taskId = i;
            executor.submit(() -> {
                System.out.println("Task " + taskId + " is running on thread: " + Thread.currentThread().getName());
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            });
        }

        executor.shutdown();
    }
}
```

#### 4. 线程同步与通信
- **锁机制**：`synchronized`关键字和`Lock`接口及其实现（如`ReentrantLock`）。
- **原子类**：`java.util.concurrent.atomic`包中的类，基于`CAS`实现。
- **`volatile`关键字**：保证可见性和禁止指令重排序。
- **`ThreadLocal`**：线程局部变量。

```java
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

// 锁机制
class Counter {
    private int count = 0;
    private final Lock lock = new ReentrantLock();

    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }

    public int getCount() {
        return count;
    }
}

// 原子类
class AtomicCounter {
    private AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet();
    }

    public int getCount() {
        return count.get();
    }
}

// volatile关键字
class VolatileExample {
    private volatile boolean flag = false;

    public void setFlag() {
        flag = true;
    }

    public boolean getFlag() {
        return flag;
    }
}

// ThreadLocal
class ThreadLocalExample {
    private static final ThreadLocal<Integer> threadLocal = new ThreadLocal<>();

    public static void setValue(int value) {
        threadLocal.set(value);
    }

    public static int getValue() {
        return threadLocal.get();
    }

    public static void removeValue() {
        threadLocal.remove();
    }
}

public class ThreadSynchronizationExample {
    public static void main(String[] args) throws InterruptedException {
        // 锁机制
        Counter counter = new Counter();
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                counter.increment();
            }
        });
        t1.start();
        t2.start();
        t1.join();
        t2.join();
        System.out.println("Counter value: " + counter.getCount());

        // 原子类
        AtomicCounter atomicCounter = new AtomicCounter();
        Thread t3 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                atomicCounter.increment();
            }
        });
        Thread t4 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                atomicCounter.increment();
            }
        });
        t3.start();
        t4.start();
        t3.join();
        t4.join();
        System.out.println("Atomic counter value: " + atomicCounter.getCount());

        // volatile关键字
        VolatileExample volatileExample = new VolatileExample();
        Thread t5 = new Thread(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            volatileExample.setFlag();
        });
        Thread t6 = new Thread(() -> {
            while (!volatileExample.getFlag()) {
                // 等待
            }
            System.out.println("Flag is set to true");
        });
        t5.start();
        t6.start();
        t5.join();
        t6.join();

        // ThreadLocal
        Thread t7 = new Thread(() -> {
            ThreadLocalExample.setValue(10);
            System.out.println("Thread 7 value: " + ThreadLocalExample.getValue());
            ThreadLocalExample.removeValue();
        });
        Thread t8 = new Thread(() -> {
            ThreadLocalExample.setValue(20);
            System.out.println("Thread 8 value: " + ThreadLocalExample.getValue());
            ThreadLocalExample.removeValue();
        });
        t7.start();
        t8.start();
        t7.join();
        t8.join();
    }
}
```

#### 5. 并发工具类
- `CountDownLatch`：倒计时门闩，一个或多个线程等待其他线程完成操作。
- `CyclicBarrier`：循环栅栏，一组线程互相等待到达某个屏障点。
- `Semaphore`：信号量，控制同时访问特定资源的线程数。
- `Exchanger`：线程间交换数据。
- `Phaser`：更灵活的屏障（Java 7+）。

```java
import java.util.concurrent.*;

public class ConcurrentUtilsExample {
    public static void main(String[] args) throws InterruptedException {
        // CountDownLatch
        CountDownLatch latch = new CountDownLatch(3);
        for (int i = 0; i < 3; i++) {
            final int taskId = i;
            new Thread(() -> {
                System.out.println("Task " + taskId + " is running");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Task " + taskId + " is completed");
                latch.countDown();
            }).start();
        }
        latch.await();
        System.out.println("All tasks are completed");

        // CyclicBarrier
        CyclicBarrier barrier = new CyclicBarrier(3, () -> System.out.println("All threads reached the barrier"));
        for (int i = 0; i < 3; i++) {
            final int taskId = i;
            new Thread(() -> {
                System.out.println("Thread " + taskId + " is waiting at the barrier");
                try {
                    barrier.await();
                } catch (InterruptedException | BrokenBarrierException e) {
                    e.printStackTrace();
                }
                System.out.println("Thread " + taskId + " passed the barrier");
            }).start();
        }

        // Semaphore
        Semaphore semaphore = new Semaphore(2);
        for (int i = 0; i < 5; i++) {
            final int taskId = i;
            new Thread(() -> {
                try {
                    semaphore.acquire();
                    System.out.println("Task " + taskId + " acquired the semaphore");
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release();
                    System.out.println("Task " + taskId + " released the semaphore");
                }
            }).start();
        }

        // Exchanger
        Exchanger<String> exchanger = new Exchanger<>();
        Thread t1 = new Thread(() -> {
            try {
                String data = "Data from Thread 1";
                String receivedData = exchanger.exchange(data);
                System.out.println("Thread 1 received: " + receivedData);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        Thread t2 = new Thread(() -> {
            try {
                String data = "Data from Thread 2";
                String receivedData = exchanger.exchange(data);
                System.out.println("Thread 2 received: " + receivedData);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        t1.start();
        t2.start();
        t1.join();
        t2.join();
    }
}
```

#### 6. 并发集合
- `ConcurrentHashMap`：线程安全的哈希表。
- `CopyOnWriteArrayList`/`CopyOnWriteArraySet`：线程安全的列表和集合。
- `BlockingQueue`及其实现类：用于线程间的阻塞队列。

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ConcurrentHashMap;
import java.util.concurrent.CopyOnWriteArrayList;
import java.util.concurrent.LinkedBlockingQueue;

public class ConcurrentCollectionExample {
    public static void main(String[] args) {
        // ConcurrentHashMap
        ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
        map.put("Apple", 1);
        map.put("Banana", 2);
        System.out.println("ConcurrentHashMap: " + map);

        // CopyOnWriteArrayList
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
        list.add("Apple");
        list.add("Banana");
        System.out.println("CopyOnWriteArrayList: " + list);

        // BlockingQueue
        BlockingQueue<String> queue = new LinkedBlockingQueue<>();
        try {
            queue.put("Apple");
            queue.put("Banana");
            System.out.println("BlockingQueue: " + queue);
            System.out.println("Poll: " + queue.take());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

#### 7. `AQS`
- `AbstractQueuedSynchronizer`：是`ReentrantLock`、`CountDownLatch`、`Semaphore`等众多同步器的基础框架。

```java
import java.util.concurrent.locks.AbstractQueuedSynchronizer;

// 自定义同步器
class MySync extends AbstractQueuedSynchronizer {
    @Override
    protected boolean tryAcquire(int arg) {
        if (compareAndSetState(0, 1)) {
            setExclusiveOwnerThread(Thread.currentThread());
            return true;
        }
        return false;
    }

    @Override
    protected boolean tryRelease(int arg) {
        if (getState() == 0) {
            throw new IllegalMonitorStateException();
        }
        setExclusiveOwnerThread(null);
        setState(0);
        return true;
    }

    @Override
    protected boolean isHeldExclusively() {
        return getState() == 1;
    }
}

// 自定义锁
class MyLock {
    private final MySync sync = new MySync();

    public void lock() {
        sync.acquire(1);
    }

    public void unlock() {
        sync.release(1);
    }
}

public class AQSExample {
    public static void main(String[] args) {
        MyLock lock = new MyLock();
        Thread t1 = new Thread(() -> {
            lock.lock();
            try {
                System.out.println("Thread 1 acquired the lock");
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
                System.out.println("Thread 1 released the lock");
            }
        });
        Thread t2 = new Thread(() -> {
            lock.lock();
            try {
                System.out.println("Thread 2 acquired the lock");
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            } finally {
                lock.unlock();
                System.out.println("Thread 2 released the lock");
            }
        });
        t1.start();
        t2.start();
    }
}
```

#### 8. `Fork/Join`框架
- 用于并行执行任务（分治思想）。

```java
import java.util.concurrent.ForkJoinPool;
import java.util.concurrent.RecursiveTask;

// 计算数组元素之和
class SumTask extends RecursiveTask<Integer> {
    private static final int THRESHOLD = 10;
    private final int[] array;
    private final int start;
    private final int end;

    public SumTask(int[] array, int start, int end) {
        this.array = array;
        this.start = start;
        this.end = end;
    }

    @Override
    protected Integer compute() {
        if (end - start <= THRESHOLD) {
            int sum = 0;
            for (int i = start; i < end; i++) {
                sum += array[i];
            }
            return sum;
        } else {
            int mid = (start + end) / 2;
            SumTask leftTask = new SumTask(array, start, mid);
            SumTask rightTask = new SumTask(array, mid, end);

            leftTask.fork();
            int rightResult = rightTask.compute();
            int leftResult = leftTask.join();

            return leftResult + rightResult;
        }
    }
}

public class ForkJoinExample {
    public static void main(String[] args) {
        int[] array = new int[100];
        for (int i = 0; i < 100; i++) {
            array[i] = i + 1;
        }

        ForkJoinPool pool = new ForkJoinPool();
        SumTask task = new SumTask(array, 0, array.length);
        int result = pool.invoke(task);
        System.out.println("Sum: " + result);
    }
}
```

#### 9. `CompletableFuture`
- 强大的异步编程工具（Java 8+），支持链式调用、组合异步操作、处理结果和异常。

```java
import java.util.concurrent.CompletableFuture;
import java.util.concurrent.ExecutionException;

public class CompletableFutureExample {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 异步任务
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return "Hello";
        });

        // 链式调用
        CompletableFuture<String> combinedFuture = future.thenApply(s -> s + " World")
                                                        .thenApply(String::toUpperCase);

        // 获取结果
        String result = combinedFuture.get();
        System.out.println(result);
    }
}
```

### 四、Java虚拟机 (Java Virtual Machine - JVM)

#### 1. 内存区域（运行时数据区）
- **程序计数器**：记录当前线程执行的字节码行号。
- **虚拟机栈**：每个方法执行时会创建一个栈帧，包含局部变量表、操作数栈、动态链接、方法出口等信息。
- **本地方法栈**：与虚拟机栈类似，用于执行本地方法。
- **堆**：是Java对象存储的主要区域，也是垃圾回收的主要区域。
- **方法区**：存储类信息、常量、静态变量等，JDK 8+为元空间（Metaspace）。
- **运行时常量池**：是方法区的一部分，用于存储编译期生成的各种字面量和符号引用。

#### 2. 垃圾回收 (Garbage Collection - GC)
- **对象存活性判断**：Java采用可达性分析算法，通过一系列的GC Roots对象作为起点，从这些节点开始向下搜索，搜索所走过的路径称为引用链，当一个对象到GC Roots没有任何引用链相连时，则证明此对象是不可用的。
- **引用类型**：包括强引用、软引用、弱引用和虚引用。
- **GC算法**：标记-清除、标记-复制、标记-整理、分代收集理论。
- **垃圾收集器**：不同的垃圾收集器适用于不同的场景，如新生代收集器（`Serial`, `ParNew`, `Parallel Scavenge`）、老年代收集器（`Serial Old`, `Parallel Old`, `CMS`）、整堆收集器（`G1`, `ZGC`, `Shenandoah`）。
- **GC日志分析**：通过设置JVM参数（如`-Xlog:gc*`或`-XX:+PrintGCDetails`）可以查看GC日志，分析GC的性能。
- **内存分配与回收策略**：对象优先在Eden区分配、大对象直接进入老年代、长期存活对象进入老年代、动态对象年龄判定、空间分配担保。

#### 3. 类加载机制 (Class Loading)
- **过程**：加载（Loading） -> 链接（Linking：验证、准备、解析） -> 初始化（Initialization）。
- **类加载器**：包括启动类加载器（`Bootstrap ClassLoader`）、扩展类加载器（`Extension ClassLoader`）、应用程序类加载器（`Application ClassLoader`/`System ClassLoader`）和自定义类加载器。
- **双亲委派模型(Parent Delegation Model)**：原理是当一个类加载器收到类加载请求时，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成，每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到顶层的启动类加载器中，只有当父加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需的类）时，子加载器才会尝试自己去加载。作用是避免重复加载和保证核心类的安全。

#### 4. JVM性能监控与调优
- **常用命令行工具**：`jps`, `jstat`, `jinfo`, `jmap`, `jstack`, `jcmd`等。
- **图形化工具**：`JConsole`, `VisualVM`, `Java Mission Control` (JMC)等。
- **内存分析工具**：`MAT` (Eclipse Memory Analyzer Tool), `JProfiler`等。
- **调优目标**：减少GC停顿时间、提高吞吐量、控制内存占用。
- **常见参数**：堆内存设置（`-Xms`, `-Xmx`）、新生代/老年代比例（`-XX:NewRatio`）、Eden/Survivor比例（`-XX:SurvivorRatio`）、元空间设置（`-XX:MetaspaceSize`, `-XX:MaxMetaspaceSize`）、选择垃圾收集器（`-XX:+UseG1GC`, `-XX:+UseZGC`等）、GC日志相关参数。

#### 5. Java内存模型 (Java Memory Model - JMM)
- **主内存与工作内存**：主内存是所有线程共享的，工作内存是每个线程独有的，线程对变量的所有操作（读取、赋值等）都必须在工作内存中进行，而不能直接读写主内存中的变量。
- **内存间交互操作**：包括`read`, `load`, `use`, `assign`, `store`, `write`, `lock`, `unlock`等。
- **`volatile`的特殊规则**：保证可见性和禁止指令重排序，但不保证原子性。
- **原子性、可见性、有序性**：是并发编程中的三个重要特性。
- **`happens-before`原则**：用于判断一个操作是否对另一个操作可见。

### 五、数据库 (Database)

#### 1. SQL基础
- **DDL（数据定义语言）**：用于定义数据库对象，如`CREATE`, `ALTER`, `DROP`等。
- **DML（数据操作语言）**：用于操作数据库中的数据，如`INSERT`, `UPDATE`, `DELETE`等。
- **DQL（数据查询语言）**：用于查询数据库中的数据，如`SELECT`。
- **DCL（数据控制语言）**：用于控制数据库的访问权限，如`GRANT`, `REVOKE`等。
- **TCL（事务控制语言）**：用于管理数据库事务，如`COMMIT`, `ROLLBACK`等。

#### 2. JDBC (Java Database Connectivity)
- **核心接口**：包括`Driver`, `Connection`, `Statement`, `PreparedStatement`, `CallableStatement`, `ResultSet`等。
- **使用步骤**：注册驱动、获取连接、创建语句对象、执行SQL、处理结果集、关闭资源。
- **事务管理**：通过`Connection`对象的`setAutoCommit()`、`commit()`和`rollback()`方法来管理事务。
- **连接池原理与使用**：连接池可以减少数据库连接的创建和销毁开销，提高性能。常见的连接池实现有`HikariCP`, `Druid`等。

```java
import java.sql.*;

public class JDBCExample {
    public static void main(String[] args) {
        String url = "jdbc:mysql://localhost:3306/test";
        String username = "root";
        String password = "password";

        try (Connection conn = DriverManager.getConnection(url, username, password);
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT * FROM users")) {

            while (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");
                System.out.println("ID: " + id + ", Name: " + name);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

#### 3. ORM框架
- **MyBatis**
    - **核心概念**：包括`SqlSessionFactory`, `SqlSession`, `Mapper`接口/XML等。
    - **动态SQL**：可以根据不同的条件生成不同的SQL语句。
    - **缓存机制**：包括一级缓存（基于`SqlSession`）和二级缓存（基于`Mapper`）。
    - **插件开发**：可以通过实现`Interceptor`接口来开发插件。
    - **与Spring集成**：可以通过Spring来管理MyBatis的组件。

```java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

// 实体类
class User {
    private int id;
    private String name;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{id=" + id + ", name='" + name + "'}";
    }
}

// Mapper接口
interface UserMapper {
    User getUserById(int id);
}

public class MyBatisExample {
    public static void main(String[] args) {
        String resource = "mybatis-config.xml";
        try (InputStream inputStream = Resources.getResourceAsStream(resource)) {
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
            try (SqlSession session = sqlSessionFactory.openSession()) {
                UserMapper userMapper = session.getMapper(UserMapper.class);
                User user = userMapper.getUserById(1);
                System.out.println(user);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- **JPA (Java Persistence API) / Hibernate**
    - **ORM概念**：通过实体类和映射注解（如`@Entity`, `@Table`, `@Column`等）将Java对象映射到数据库表。
    - **`EntityManager`**：用于管理实体的生命周期，如持久化、查询、删除等。
    - **JPA Repository (Spring Data JPA)**：提供了简单的CRUD操作和自定义查询方法。
    - **对象关系映射策略**：包括一对一、一对多、多对一、多对多等关系的映射。
    - **Hibernate缓存**：包括一级Session缓存、二级缓存和查询缓存。
    - **Hibernate与MyBatis对比**：Hibernate是全自动的ORM框架，提供了更高级的功能和更简洁的代码；MyBatis是半自动的ORM框架，更灵活，适合对SQL有较高要求的场景。

```java
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.Persistence;

// 实体类
@Entity
class Product {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Product{id=" + id + ", name='" + name + "'}";
    }
}

public class JPAExample {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("myPU");
        EntityManager em = emf.createEntityManager();

        try {
            em.getTransaction().begin();
            Product product = new Product();
            product.setName("Test Product");
            em.persist(product);
            em.getTransaction().commit();

            Product retrievedProduct = em.find(Product.class, product.getId());
            System.out.println(retrievedProduct);
        } finally {
            em.close();
            emf.close();
        }
    }
}
```

#### 4. 数据库连接池
- **原理**：连接池在初始化时会创建一定数量的数据库连接，当应用程序需要使用数据库连接时，从连接池中获取一个空闲的连接，使用完毕后将连接返回给连接池，而不是直接关闭连接。
- **优势**：减少数据库连接的创建和销毁开销，提高性能；可以对连接进行管理和监控。
- **常见实现**：`HikariCP`是高性能的默认选择，`Druid`功能强大，带有监控功能。

```java
import com.zaxxer.hikari.HikariConfig;
import com.zaxxer.hikari.HikariDataSource;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.Statement;

public class ConnectionPoolExample {
    public static void main(String[] args) {
        HikariConfig config = new HikariConfig();
        config.setJdbcUrl("jdbc:mysql://localhost:3306/test");
        config.setUsername("root");
        config.setPassword("password");
        config.setMaximumPoolSize(10);

        HikariDataSource dataSource = new HikariDataSource(config);

        try (Connection conn = dataSource.getConnection();
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery("SELECT * FROM users")) {

            while (rs.next()) {
                int id = rs.getInt("id");
                String name = rs.getString("name");
                System.out.println("ID: " + id + ", Name: " + name);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 六、开发框架 (Development Frameworks)

#### 1. Spring Framework
- **IoC (Inversion of Control) / DI (Dependency Injection)**
    - **核心概念**：IoC是一种设计原则，将对象的创建和依赖关系的管理交给容器来完成；DI是IoC的具体实现方式，通过构造器注入、Setter注入、字段注入等方式将依赖对象注入到目标对象中。
    - **`BeanFactory` vs `ApplicationContext`**：`BeanFactory`是Spring的基础容器，`ApplicationContext`是`BeanFactory`的子接口，提供了更多的功能，如国际化支持、事件发布等。
    - **Bean作用域**：包括`singleton`（单例）、`prototype`（原型）等。
    - **Bean生命周期**：包括实例化、属性赋值、初始化、销毁等阶段。
    - **依赖注入方式**：构造器注入、Setter注入、字段注入（不推荐）。
    - **注解**：`@Autowired`, `@Resource`, `@Component`及相关注解（`@Service`, `@Repository`, `@Controller`）。

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

// 服务类
@Component
class UserService {
    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public void doSomething() {
        userRepository.saveUser();
    }
}

// 仓库类
@Component
class UserRepository {
    public void saveUser() {
        System.out.println("User saved");
    }
}

// 主类
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;

@ComponentScan
public class SpringIoCExample {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(SpringIoCExample.class);
        UserService userService = context.getBean(UserService.class);
        userService.doSomething();
    }
}
```

- **AOP (Aspect-Oriented Programming)**
    - **核心概念**：包括切面（Aspect）、连接点（Joinpoint）、通知（Advice）、切点（Pointcut）、引入（Introduction）、织入（Weaving）等。
    - **通知类型**：`@Before`, `@After`, `@AfterReturning`, `@AfterThrowing`, `@Around`。
    - **实现原理**：基于动态代理（JDK Proxy vs CGLIB）。

```java
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.stereotype.Component;

// 切面类
@Aspect
@Component
class LoggingAspect {
    @Pointcut("execution(* com.example.UserService.*(..))")
    public void userServiceMethods() {}

    @Before("userServiceMethods()")
    public void beforeAdvice(JoinPoint joinPoint) {
        System.out.println("Before method: " + joinPoint.getSignature().getName());
    }

    @After("userServiceMethods()")
    public void afterAdvice(JoinPoint joinPoint) {
        System.out.println("After method: " + joinPoint.getSignature().getName());
    }
}

// 主类
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@EnableAspectJAutoProxy
@ComponentScan
public class SpringAOPExample {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(SpringAOPExample.class);
        UserService userService = context.getBean(UserService.class);
        userService.doSomething();
    }
}
```

- **事务管理**
    - **声明式事务**：通过`@Transactional`注解来管理事务。
    - **传播行为**：定义了事务在不同方法调用时的传播方式，如`Propagation.REQUIRED`, `Propagation.REQUIRES_NEW`等。
    - **隔离级别**：定义了事务之间的隔离程度，如`Isolation.READ_COMMITTED`, `Isolation.SERIALIZABLE`等。
    - **回滚规则**：可以指定哪些异常会导致事务回滚。

```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

// 服务类
@Service
class TransactionalService {
    private final UserRepository userRepository;

    @Autowired
    public TransactionalService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Transactional
    public void doTransactionalOperation() {
        userRepository.saveUser();
        // 模拟异常
        throw new RuntimeException("Transaction failed");
    }
}

// 主类
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.EnableTransactionManagement;

@EnableTransactionManagement
@ComponentScan
public class SpringTransactionExample {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(SpringTransactionExample.class);
        TransactionalService transactionalService = context.getBean(TransactionalService.class);
        try {
            transactionalService.doTransactionalOperation();
        } catch (Exception e) {
            System.out.println("Exception caught: " + e.getMessage());
        }
    }
}
```

- **Spring MVC**
    - **核心组件**：包括`DispatcherServlet`, `HandlerMapping`, `HandlerAdapter`, `ViewResolver`等。
    - **请求处理流程**：客户端发送请求到`DispatcherServlet`，`DispatcherServlet`根据`HandlerMapping`找到对应的处理器，通过`HandlerAdapter`调用处理器方法，处理器方法返回`ModelAndView`对象，`DispatcherServlet`根据`ViewResolver`将`ModelAndView`对象解析为视图并返回给客户端。
    - **常用注解**：`@Controller`, `@RestController`, `@RequestMapping`, `@RequestParam`, `@PathVariable`, `@RequestBody`, `@ResponseBody`等。
    - **数据绑定与验证**：可以将请求参数绑定到Java对象中，并进行数据验证。
    - **文件上传下载**：可以通过`MultipartFile`实现文件上传，通过`ResponseEntity`实现文件下载。
    - **拦截器**：可以通过实现`HandlerInterceptor`接口来定义拦截器，对请求进行预处理和后处理。

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

// 控制器类
@Controller
public class HelloController {
    @GetMapping("/hello")
    @ResponseBody
    public String hello(@RequestParam(name = "name", defaultValue = "World") String name) {
        return "Hello, " + name + "!";
    }
}

// 主类
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringMVCExample {
    public static void main(String[] args) {
        SpringApplication.run(SpringMVCExample.class, args);
    }
}
```

- **Spring核心模块**：包括Spring Core, Spring Beans, Spring Context, Spring AOP, Spring Expression Language (SpEL)等。

#### 2. Spring Boot
- **核心特性**
    - **自动配置**：通过`@EnableAutoConfiguration`和`spring.factories`文件实现自动配置，减少了开发人员的配置工作。
    - **起步依赖**：提供了一系列的Starter依赖，方便开发人员快速集成各种功能。
    - **嵌入式Web服务器**：支持Tomcat, Jetty, Undertow等嵌入式Web服务器。
    - **Actuator**：提供了应用监控与管理的功能。
    - **外部化配置**：可以通过`application.properties`/`application.yml`文件和Profile来进行外部化配置。
- **简化Spring开发**：采用约定优于配置的原则，零XML配置（Java Config）。
- **常用注解**：`@SpringBootApplication`。

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@SpringBootApplication
public class SpringBootExample {
    @GetMapping("/")
    public String hello() {
        return "Hello, Spring Boot!";
    }

    public static void main(String[] args) {
        SpringApplication.run(SpringBootExample.class, args);
    }
}
```

#### 3. Spring Cloud (微服务)
- **服务注册与发现**：常用的服务注册中心有Eureka, Nacos, Consul等。
- **服务调用**：可以使用`RestTemplate`或`OpenFeign`进行服务调用。
- **负载均衡**：可以使用`Ribbon`或`Spring Cloud LoadBalancer`进行负载均衡。
- **服务熔断与降级**：常用的服务熔断与降级框架有Hystrix, Sentinel, Resilience4j等。
- **服务网关**：可以使用Zuul或`Spring Cloud Gateway`作为服务网关。
- **配置中心**：可以使用`Spring Cloud Config`或Nacos Config作为配置中心。
- **分布式链路追踪**：可以使用Sleuth + Zipkin进行分布式链路追踪。
- **消息驱动**：可以使用`Spring Cloud Stream`进行消息驱动开发。
- **分布式事务**：可以使用Seata进行分布式事务管理。

### 七、系统设计与工具 (System Design & Tools)

#### 1. 常用设计模式
- **创建型**：包括单例模式、工厂模式、抽象工厂模式、建造者模式、原型模式等。
- **结构型**：包括适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式等。
- **行为型**：包括策略模式、模板方法模式、观察者模式、迭代器模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式等。

```java
// 单例模式（懒汉式，线程安全）
class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

// 工厂模式
interface Shape {
    void draw();
}

class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a circle");
    }
}

class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a rectangle");
    }
}

class ShapeFactory {
    public Shape getShape(String shapeType) {
        if (shapeType == null) {
            return null;
        }
        if (shapeType.equalsIgnoreCase("CIRCLE")) {
            return new Circle();
        } else if (shapeType.equalsIgnoreCase("RECTANGLE")) {
            return new Rectangle();
        }
        return null;
    }
}

// 主类
public class DesignPatternExample {
    public static void main(String[] args) {
        // 单例模式
        Singleton singleton = Singleton.getInstance();
        System.out.println(singleton);

        // 工厂模式
        ShapeFactory factory = new ShapeFactory();
        Shape circle = factory.getShape("CIRCLE");
        circle.draw();
        Shape rectangle = factory.getShape("RECTANGLE");
        rectangle.draw();
    }
}
```

#### 2. 数据结构与算法基础
- **数据结构**：包括数组、链表、栈、队列、树（二叉树、二叉搜索树、AVL树、红黑树）、堆、图、哈希表等。
- **常用算法**：包括排序（冒泡、选择、插入、希尔、归并、快速、堆排序）、查找、递归、DFS/BFS、动态规划、贪心算法等。
- **Big O表示法**：用于描述算法的时间复杂度和空间复杂度。

```java
// 快速排序
public class QuickSort {
    public static void quickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pivotIndex = partition(arr, low, high);
            quickSort(arr, low, pivotIndex - 1);
            quickSort(arr, pivotIndex + 1, high);
        }
    }

    private static int partition(int[] arr, int low, int high) {
        int pivot = arr[high];
        int i = low - 1;
        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                swap(arr, i, j);
            }
        }
        swap(arr, i + 1, high);
        return i + 1;
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public static void main(String[] args) {
        int[] arr = {5, 3, 8, 4, 2};
        quickSort(arr, 0, arr.length - 1);
        for (int num : arr) {
            System.out.print(num + " ");
        }
    }
}
```

#### 3. 网络基础
- **OSI七层模型/TCP/IP四层模型**：OSI七层模型包括物理层、数据链路层、网络层、传输层、会话层、表示层、应用层；TCP/IP四层模型包括网络接口层、网络层、传输层、应用层。
- **TCP vs UDP**：TCP是面向连接的、可靠的传输协议，UDP是无连接的、不可靠的传输协议。
- **三次握手与四次挥手**：TCP建立连接时需要进行三次握手，断开连接时需要进行四次挥手。
- **HTTP/HTTPS协议**：HTTP是超文本传输协议，HTTPS是在HTTP的基础上加入了SSL/TLS协议，保证了数据传输的安全性。
- **WebSocket**：是一种在单个TCP连接上进行全双工通信的协议，适用于实时通信场景。

#### 4. Linux常用命令
- **文件操作**：`ls`, `cd`, `cp`, `mv`, `rm`, `mkdir`, `cat`, `more`, `less`, `tail`, `head`, `grep`, `find`等。
- **系统管理**：`ps`, `top`, `kill`, `df`, `du`, `free`, `netstat`, `ifconfig`等。
- **权限管理**：`chmod`, `chown`, `chgrp`等。
- **文本处理**：`sed`, `awk`等。
- **压缩解压**：`tar`, `gzip`, `zip/unzip`等。
- **网络工具**：`ping`, `telnet`, `curl`, `wget`等。

#### 5. 构建工具
- **Maven**
    - **POM文件结构**：包括项目的基本信息、依赖管理、插件配置等。
    - **坐标(GAV)**：包括groupId, artifactId, version，用于唯一标识一个项目。
    - **依赖管理**：包括依赖范围（scope）、传递依赖、依赖冲突解决等。
    - **仓库**：包括本地仓库、远程仓库、中央仓库。
    - **生命周期与插件**：Maven有三个主要的生命周期（clean, default, site），可以通过插件来扩展功能。
    - **多模块项目**：可以将一个大型项目拆分成多个模块，方便管理和维护。
- **Gradle**
    - **基于Groovy/DSL**：Gradle使用Groovy或Kotlin DSL来编写构建脚本。
    - **构建脚本(`build.gradle`)**：包括项目的基本信息、依赖管理、任务定义等。
    - **依赖管理**：与Maven类似，支持传递依赖和依赖冲突解决。
    - **任务定义**：可以自定义任务，实现自动化构建。

#### 6. 版本控制
- **Git**
    - **核心概念**：包括工作区、暂存区、版本库。
    - **常用命令**：`git init/clone/add/commit/status/diff/log/show/push/pull/fetch/branch/checkout/merge/rebase/stash/tag`等。
    - **分支管理策略**：如Git Flow，用于规范分支的创建、合并和删除。
    - **远程仓库操作**：可以使用GitHub, GitLab, Gitee等远程仓库进行代码托管。

#### 7. 集成开发环境 (IDE)
- **IntelliJ IDEA**：功能强大，支持多种开发语言和框架，提供了丰富的插件和工具。
- **Eclipse**：开源的集成开发环境，广泛应用于Java开发。

面试准备部分是为求职者提供在 Java 相关面试中可能会遇到的各类问题以及项目经验阐述方法，以下为你详细展开讲解各部分内容：

### 1. Java 基础面试题
- **高频基础概念**
    - **数据类型**：要清楚基本数据类型（如 `int`、`double`、`boolean` 等）和引用数据类型（类、接口、数组）的区别，以及它们的默认值。例如，`int` 的默认值是 0，而引用类型的默认值是 `null`。
    - **运算符**：理解不同运算符的优先级和结合性，如算术运算符、逻辑运算符、位运算符等。例如，`&&` 和 `&` 的区别，`&&` 具有短路特性。
    - **流程控制**：掌握 `if - else`、`switch`、`for`、`while`、`do - while` 等语句的使用场景和执行流程。例如，`switch` 语句在 JDK 7 之后可以使用字符串作为表达式。
- **OOP（面向对象编程）**
    - **核心概念**：类与对象是 OOP 的基础，要能解释类是对象的抽象，对象是类的实例。封装是将数据和操作数据的方法绑定在一起，隐藏内部实现细节；继承是子类继承父类的属性和方法，实现代码复用；多态是同一个方法可以根据对象的不同类型表现出不同的行为；抽象是将一类对象的共同特征提取出来形成抽象类或接口。
    - **关键机制**：构造方法用于创建对象时初始化对象的状态；方法重载是指在同一个类中，方法名相同但参数列表不同；方法重写是子类重写父类的方法，要求方法名、参数列表和返回值类型都相同；访问权限控制符决定了类、方法和变量的访问范围。
    - **抽象类与接口**：抽象类可以包含抽象方法和非抽象方法，而接口只能包含抽象方法（JDK 8 之后可以有默认方法和静态方法）。抽象类适用于需要共享代码和部分实现的场景，接口适用于定义规范和行为的场景。
    - `this` 和 `super` 关键字：`this` 关键字指向当前对象，用于区分成员变量和局部变量；`super` 关键字指向父类对象，用于调用父类的方法和构造方法。
- **集合**：了解 `Collection` 接口下的 `List`、`Set`、`Queue` 以及 `Map` 接口的特点和常用实现类。例如，`List` 是有序可重复的，`Set` 是无序不可重复的，`Map` 是键值对的集合。
- **异常**：清楚 `Throwable` 类的层次结构，包括 `Error` 和 `Exception`。区分 Checked Exception 和 Unchecked Exception（RuntimeException），掌握 `try - catch - finally` 和 `try - with - resources` 语句的使用，以及如何自定义异常。
- **泛型**：理解泛型的概念和优势，如类型安全、消除强制转换。掌握通配符（`?`、`? extends T`、`? super T`）的使用和类型擦除的原理。
- **反射**：知道如何使用 `Class` 类获取类的信息，动态创建对象、调用方法和访问字段。反射在框架开发中应用广泛，如 Spring 框架的依赖注入。
- **新特性**：了解 Java 8 及之后的新特性，如 Lambda 表达式、Stream API、方法引用、接口默认方法和静态方法、日期时间 API 等。

### 2. 集合面试题
- **`HashMap`/`ConcurrentHashMap` 原理**
    - **`HashMap`**：基于哈希表实现，通过键的哈希值计算数组索引，使用链表或红黑树解决哈希冲突。当链表长度超过 8 且数组长度大于 64 时，链表会转换为红黑树；当红黑树节点数小于 6 时，红黑树会转换为链表。要了解其扩容机制，当元素数量达到阈值（容量 * 负载因子）时，会进行扩容操作，扩容为原来的 2 倍。
    - **`ConcurrentHashMap`**：是线程安全的哈希表，在 JDK 7 中采用分段锁机制，将整个哈希表分成多个段，每个段是一个独立的小哈希表，不同段可以同时进行读写操作；在 JDK 8 中采用 CAS + synchronized 机制，通过 CAS 操作保证并发插入和更新，使用 synchronized 对链表或红黑树的头节点加锁，减少锁的粒度。
- **`ArrayList` vs `LinkedList`**
    - **`ArrayList`**：基于数组实现，随机访问快，时间复杂度为 O(1)，但插入和删除操作慢，需要移动元素，时间复杂度为 O(n)。
    - **`LinkedList`**：基于双向链表实现，插入和删除操作快，时间复杂度为 O(1)，但随机访问慢，需要遍历链表，时间复杂度为 O(n)。
- **`HashSet` 原理**：基于 `HashMap` 实现，`HashSet` 中的元素存储在 `HashMap` 的键中，值统一为一个静态常量对象。`HashSet` 利用 `HashMap` 的哈希冲突解决机制来保证元素的唯一性。
- **`fail - fast` vs `fail - safe`**
    - **`fail - fast`**：当在迭代集合时，如果集合的结构被修改（如添加、删除元素），会立即抛出 `ConcurrentModificationException` 异常。`ArrayList`、`HashMap` 等集合采用 `fail - fast` 机制。
    - **`fail - safe`**：在迭代集合时，会复制一份集合的快照进行迭代，即使集合的结构被修改，也不会抛出异常。`CopyOnWriteArrayList`、`ConcurrentHashMap` 等集合采用 `fail - safe` 机制。

### 3. 并发面试题
- **线程状态**：了解线程的生命周期和状态，包括 `NEW`（新建）、`RUNNABLE`（可运行）、`BLOCKED`（阻塞）、`WAITING`（等待）、`TIMED_WAITING`（定时等待）、`TERMINATED`（终止）。知道线程状态之间的转换条件，如调用 `start()` 方法使线程从 `NEW` 状态转换为 `RUNNABLE` 状态，调用 `wait()` 方法使线程从 `RUNNABLE` 状态转换为 `WAITING` 状态。
- **线程创建**：掌握三种线程创建方式，继承 `Thread` 类、实现 `Runnable` 接口和实现 `Callable` 接口 + `Future`/`FutureTask`。了解它们的优缺点，如继承 `Thread` 类会限制类的继承，而实现 `Runnable` 接口和 `Callable` 接口更灵活。
- **线程池**：理解线程池的核心原理，包括 `ThreadPoolExecutor` 的核心参数（核心线程数、最大线程数、工作队列、拒绝策略、线程工厂、存活时间）。知道 `Executors` 工厂类的常用方法（如 `newFixedThreadPool`、`newCachedThreadPool` 等）及其缺点，推荐手动创建 `ThreadPoolExecutor` 以避免资源耗尽问题。
- **`synchronized`/`Lock`**
    - **`synchronized`**：是 Java 内置的锁机制，可以修饰方法和代码块。修饰方法时，锁对象是当前对象或类对象；修饰代码块时，需要指定锁对象。`synchronized` 基于监视器锁（Monitor）实现，是可重入锁。
    - **`Lock`**：是一个接口，常用的实现类有 `ReentrantLock`。`Lock` 提供了更灵活的锁操作，如可中断锁、定时锁、公平锁等。
- **`volatile`**：保证变量的可见性，即一个线程修改了 `volatile` 变量的值，其他线程能立即看到最新值。同时，`volatile` 禁止指令重排序，但不保证原子性。
- **`CAS`**：即 Compare - And - Swap，是一种无锁算法。`CAS` 操作包含三个操作数：内存位置（V）、预期原值（A）和新值（B）。如果内存位置的值与预期原值相匹配，那么处理器会自动将该位置值更新为新值；否则，处理器不做任何操作。`CAS` 在 `AtomicInteger`、`AtomicReference` 等原子类中广泛应用。
- **`AQS`**：`AbstractQueuedSynchronizer` 是一个抽象队列同步器，是许多同步器（如 `ReentrantLock`、`CountDownLatch`、`Semaphore` 等）的基础框架。`AQS` 内部使用一个 `int` 类型的状态变量和一个双向队列来实现锁和同步机制。
- **`ThreadLocal`**：是线程局部变量，每个使用 `ThreadLocal` 的线程都有自己独立的副本，线程之间互不影响。`ThreadLocal` 常用于保存线程上下文信息，如用户信息、事务信息等。
- **并发工具类**：了解 `CountDownLatch`、`CyclicBarrier`、`Semaphore`、`Exchanger`、`Phaser` 等并发工具类的作用和使用场景。例如，`CountDownLatch` 用于一个或多个线程等待其他线程完成操作，`CyclicBarrier` 用于一组线程互相等待到达某个屏障点。

### 4. JVM 面试题
- **内存区域**：清楚 JVM 的运行时数据区，包括程序计数器、虚拟机栈、本地方法栈、堆、方法区（JDK 8 之后为元空间）和运行时常量池。了解每个区域的作用和特点，如堆是 GC 主要区域，存放对象实例；虚拟机栈用于存储局部变量表、操作数栈等。
- **垃圾回收**
    - **对象存活性判断**：知道引用计数法和可达性分析算法，Java 采用可达性分析算法，通过 GC Roots 来判断对象是否可达。
    - **引用类型**：掌握强引用、软引用、弱引用和虚引用的区别和应用场景。强引用是最常见的引用类型，只要强引用存在，对象就不会被回收；软引用在内存不足时会被回收；弱引用在下次垃圾回收时会被回收；虚引用主要用于跟踪对象被垃圾回收的状态。
    - **GC 算法**：了解标记 - 清除、标记 - 复制、标记 - 整理、分代收集理论等 GC 算法的原理和优缺点。
    - **垃圾收集器**：熟悉不同的垃圾收集器，如新生代收集器（`Serial`、`ParNew`、`Parallel Scavenge`）、老年代收集器（`Serial Old`、`Parallel Old`、`CMS`）和整堆收集器（`G1`、`ZGC`、`Shenandoah`）的特点和适用场景。
    - **GC 日志分析**：知道如何使用 `-Xlog:gc*` 或 `-XX:+PrintGCDetails` 等参数来开启 GC 日志，并能分析日志中的信息，如垃圾回收的时间、内存使用情况等。
    - **内存分配与回收策略**：了解对象优先在 Eden 区分配、大对象直接进入老年代、长期存活对象进入老年代、动态对象年龄判定、空间分配担保等内存分配与回收策略。
- **类加载机制**
    - **过程**：掌握类加载的过程，包括加载、链接（验证、准备、解析）和初始化。了解每个阶段的主要任务，如加载阶段将类的字节码文件加载到内存中，初始化阶段执行类的静态代码块和静态变量的赋值操作。
    - **类加载器**：清楚 `Bootstrap ClassLoader`、`Extension ClassLoader`、`Application ClassLoader` 和自定义类加载器的作用和加载范围。
    - **双亲委派模型**：理解双亲委派模型的原理和作用，即类加载器在加载类时，先委托给父类加载器加载，只有父类加载器无法加载时，才由自己加载。双亲委派模型可以避免重复加载和保证核心类的安全。了解破坏双亲委派模型的场景，如 SPI 机制和 OSGi。
- **JMM（Java 内存模型）**：了解 JMM 的主内存与工作内存的概念，以及内存间交互操作（`read`、`load`、`use`、`assign`、`store`、`write`、`lock`、`unlock`）。掌握 `volatile` 的特殊规则（可见性、禁止重排序），以及原子性、可见性、有序性的概念和 `happens - before` 原则。

### 5. 数据库面试题
- **SQL 优化**：掌握 SQL 优化的方法，如合理使用索引、避免全表扫描、优化查询语句结构、减少子查询等。例如，在经常用于查询条件和排序的字段上创建索引。
- **索引原理（B + 树）**：了解 B + 树的结构和特点，B + 树是一种平衡的多路搜索树，所有数据都存储在叶子节点上，叶子节点之间通过指针相连。B + 树适用于数据库索引，因为它可以提高查询效率，减少磁盘 I/O 次数。
- **事务特性（ACID）**：清楚事务的四个特性，原子性（Atomicity）表示事务中的操作要么全部执行，要么全部不执行；一致性（Consistency）表示事务执行前后数据库的状态保持一致；隔离性（Isolation）表示多个事务之间相互隔离，互不影响；持久性（Durability）表示事务一旦提交，其结果将永久保存在数据库中。
- **隔离级别**：了解数据库的四种隔离级别，读未提交（Read Uncommitted）、读已提交（Read Committed）、可重复读（Repeatable Read）和串行化（Serializable）。不同的隔离级别会导致不同的并发问题，如脏读、不可重复读和幻读。
- **锁**：掌握行锁、表锁、间隙锁、死锁的概念和使用场景。行锁是对一行数据加锁，表锁是对整个表加锁，间隙锁用于解决幻读问题，死锁是指两个或多个事务在执行过程中，因争夺锁资源而造成的一种互相等待的现象。
- **MVCC（多版本并发控制）**：了解 MVCC 的原理和作用，MVCC 是一种并发控制技术，通过为数据的每个版本添加时间戳或版本号，实现多事务的并发访问，提高数据库的并发性能。
- **`explain`**：知道如何使用 `explain` 关键字来分析 SQL 语句的执行计划，了解执行计划中的各个字段的含义，如 `id`、`select_type`、`table`、`type`、`possible_keys`、`key`、`key_len`、`ref`、`rows`、`Extra` 等，通过分析执行计划可以找出 SQL 语句的性能瓶颈。
- **分库分表**：了解分库分表的概念和原因，当数据库的数据量和并发量达到一定程度时，需要进行分库分表来提高数据库的性能和可扩展性。掌握垂直分库分表和水平分库分表的方法和优缺点。
- **主从复制、读写分离**：了解主从复制的原理和实现方式，主从复制是指将主数据库的数据复制到从数据库，实现数据的备份和读写分离。读写分离是指将读操作和写操作分别分配到不同的数据库服务器上，提高数据库的并发性能。

### 6. 框架面试题
- **Spring IoC/AOP 原理**
    - **IoC（控制反转）**：理解 IoC 的核心概念，即对象的创建和依赖关系的管理由 Spring 容器负责，而不是由对象本身负责。掌握 `BeanFactory` 和 `ApplicationContext` 的区别，`ApplicationContext` 是 `BeanFactory` 的子接口，提供了更多的功能，如国际化支持、事件发布等。了解 Bean 的作用域（`singleton`、`prototype` 等）和生命周期，以及依赖注入的方式（构造器注入、Setter 注入、字段注入）。
    - **AOP（面向切面编程）**：掌握 AOP 的核心概念，如切面（Aspect）、连接点（Joinpoint）、通知（Advice）、切点（Pointcut）、引入（Introduction）、织入（Weaving）。了解通知的类型（`@Before`、`@After`、`@AfterReturning`、`@AfterThrowing`、`@Around`）和 AOP 的实现原理（动态代理 - JDK Proxy vs CGLIB）。
- **Bean 生命周期**：清楚 Bean 的生命周期，包括实例化、属性赋值、初始化、使用和销毁等阶段。了解在每个阶段可以使用的回调方法，如 `InitializingBean` 接口的 `afterPropertiesSet()` 方法和 `DisposableBean` 接口的 `destroy()` 方法。
- **事务传播行为**：掌握 Spring 事务的传播行为，如 `Propagation.REQUIRED`（如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务）、`Propagation.REQUIRES_NEW`（总是创建一个新的事务，如果当前存在事务，则将当前事务挂起）等。了解不同传播行为的应用场景。
- **Spring MVC 流程**：熟悉 Spring MVC 的核心组件（`DispatcherServlet`、`HandlerMapping`、`HandlerAdapter`、`ViewResolver` 等）和请求处理流程。当客户端发送请求时，`DispatcherServlet` 接收请求，通过 `HandlerMapping` 找到对应的处理器，然后通过 `HandlerAdapter` 调用处理器的方法，最后通过 `ViewResolver` 解析视图并返回给客户端。
- **Spring Boot 自动配置原理**：了解 Spring Boot 的自动配置原理，基于 `@EnableAutoConfiguration` 注解和 `spring.factories` 文件，Spring Boot 会根据类路径下的依赖和配置自动配置 Spring 应用。掌握起步依赖（Starter）的作用，起步依赖是一组预定义的依赖集合，可以简化项目的依赖管理。
- **MyBatis 缓存**：了解 MyBatis 的一级缓存和二级缓存的原理和使用场景。一级缓存是基于 SqlSession 的，同一个 SqlSession 中执行相同的查询语句会从缓存中获取结果；二级缓存是基于 Mapper 的，多个 SqlSession 可以共享二级缓存。
- **#{} vs ${}**：掌握 `#{}` 和 `${}` 在 MyBatis 中的区别，`#{}` 是预编译的占位符，会防止 SQL 注入；`${}` 是字符串替换，会直接将参数值替换到 SQL 语句中，可能会导致 SQL 注入问题。

### 7. 系统设计面试题
- **设计模式应用**：掌握常用设计模式的意图、结构和应用场景，如单例模式用于确保一个类只有一个实例，工厂模式用于创建对象，观察者模式用于实现对象之间的一对多依赖关系等。能够在实际项目中灵活运用设计模式来解决问题。
- **缓存策略（Redis/Memcached）**：了解 Redis 和 Memcached 的特点和区别，Redis 支持多种数据结构（如字符串、哈希、列表、集合、有序集合），支持持久化，而 Memcached 只支持简单的键值对存储，不支持持久化。掌握缓存的使用场景和缓存更新策略，如缓存穿透、缓存雪崩、缓存击穿等问题的解决方案。
- **消息队列（Kafka/RabbitMQ/RocketMQ）**：了解 Kafka、RabbitMQ 和 RocketMQ 的特点和适用场景，Kafka 适用于大数据场景，具有高吞吐量、分布式、持久化等特点；RabbitMQ 适用于企业级应用，支持多种消息协议和高级特性；RocketMQ 是阿里巴巴开源的消息队列，具有高性能、高可用性、分布式等特点。掌握消息队列的使用场景，如异步处理、解耦、流量削峰等。
- **分布式 ID 生成**：了解分布式 ID 生成的方法和要求，如唯一性、高性能、趋势递增等。掌握常见的分布式 ID 生成算法，如 UUID、数据库自增 ID、Snowflake 算法等。
- **分布式锁**：了解分布式锁的概念和使用场景，如在分布式系统中保证数据的一致性和并发控制。掌握常见的分布式锁实现方式，如基于 Redis 的分布式锁、基于 ZooKeeper 的分布式锁等。
- **限流熔断降级**：了解限流、熔断和降级的概念和作用，限流用于限制系统的并发访问量，熔断用于在系统出现故障时快速失败，降级用于在系统资源不足时牺牲部分功能来保证核心功能的可用性。掌握常见的限流算法，如令牌桶算法、漏桶算法等，以及熔断和降级的实现方式，如 Hystrix、Sentinel 等。
- **CAP/BASE 理论**：了解 CAP 理论（一致性、可用性、分区容错性）和 BASE 理论（基本可用、软状态、最终一致性）的概念和关系。在分布式系统中，由于网络分区的存在，无法同时满足 CAP 理论的三个特性，通常需要在一致性和可用性之间进行权衡。BASE 理论是对 CAP 理论的一种妥协，强调系统的可用性和最终一致性。
- **微服务拆分原则**：掌握微服务拆分的原则，如单一职责原则、高内聚低耦合原则、业务边界清晰原则等。能够根据业务需求和系统架构合理拆分微服务，提高系统的可维护性和可扩展性。

### 8. 项目经验
- **STAR 法则**：使用 STAR 法则来描述项目，STAR 分别代表 Situation（情景）、Task（任务）、Action（行动）和 Result（结果）。在描述项目时，先介绍项目的背景和目标（Situation），然后说明自己在项目中承担的任务和职责（Task），接着阐述为完成任务采取的具体行动和措施（Action），最后说明项目取得的成果和收益（Result）。
- **突出技术难点与解决方案**：在描述项目时，要突出项目中遇到的技术难点和挑战，并详细说明自己是如何解决这些问题的。例如，在处理高并发问题时，采用了哪些技术手段（如缓存、限流、异步处理等）来提高系统的性能和可用性。通过突出技术难点和解决方案，可以展示自己的技术能力和解决问题的能力。