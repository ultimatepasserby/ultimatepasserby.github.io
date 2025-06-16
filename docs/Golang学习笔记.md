---
title: "Golang学习笔记"
date: 2025-06-15  # 可选，手动指定日期
layout: default      # 可选，指定布局（如 `post` 或 `default`）
categories: [Golang]  # 可选，添加分类
---
### 一、基础语法
#### 1. 变量与常量
- **变量声明**：
    - 标准格式：`var 变量名 类型`
    - 简短格式：`变量名 := 值`（函数内使用）
    - 批量声明：`var (a int; b string)`
```go
package main

import "fmt"

func main() {
    // 标准格式
    var num int
    num = 10
    // 简短格式
    str := "Hello, Golang!"
    // 批量声明
    var (
        a int
        b string
    )
    a = 20
    b = "World"

    fmt.Println(num, str, a, b)
}
```
- **常量**：使用 `const` 定义，支持 `iota` 实现枚举自增。
```go
package main

import "fmt"

const (
    Monday = iota + 1
    Tuesday
    Wednesday
    Thursday
    Friday
    Saturday
    Sunday
)

func main() {
    fmt.Println(Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday)
}
```
- **数据类型**：
    - 基本类型：整型（`int8~int64`）、浮点型（`float32/64`）、布尔型（`bool`）、字符串（`string`，不可变）。
    - 派生类型：数组、切片、Map、结构体、指针等。
```go
package main

import "fmt"

func main() {
    // 基本类型
    var num int8 = 127
    var f float32 = 3.14
    var isTrue bool = true
    var str string = "Golang"

    // 派生类型 - 数组
    var arr [3]int = [3]int{1, 2, 3}
    // 派生类型 - 切片
    slice := []int{4, 5, 6}
    // 派生类型 - Map
    m := map[string]int{"one": 1, "two": 2}
    // 派生类型 - 结构体
    type Person struct {
        Name string
        Age  int
    }
    p := Person{Name: "John", Age: 30}
    // 派生类型 - 指针
    ptr := &num

    fmt.Println(num, f, isTrue, str, arr, slice, m, p, *ptr)
}
```

#### 2. 流程控制
- **分支**：
    - `if-else` 条件灵活，可前置执行语句；
    - `switch` 支持多值匹配，默认 `break`，需显式使用 `fallthrough` 继续下一分支。
```go
package main

import "fmt"

func main() {
    // if-else
    if num := 10; num > 5 {
        fmt.Println("num is greater than 5")
    } else {
        fmt.Println("num is less than or equal to 5")
    }

    // switch
    day := 3
    switch day {
    case 1, 2, 3, 4, 5:
        fmt.Println("Weekday")
    case 6, 7:
        fmt.Println("Weekend")
    default:
        fmt.Println("Invalid day")
    }

    // fallthrough
    num := 2
    switch num {
    case 1:
        fmt.Println("One")
    case 2:
        fmt.Println("Two")
        fallthrough
    case 3:
        fmt.Println("Three")
    }
}
```
- **循环**：仅 `for` 关键字，支持 `break`/`continue` 控制嵌套循环。
```go
package main

import "fmt"

func main() {
    // 基本 for 循环
    for i := 0; i < 5; i++ {
        fmt.Print(i, " ")
    }
    fmt.Println()

    // 类似 while 循环
    j := 0
    for j < 5 {
        fmt.Print(j, " ")
        j++
    }
    fmt.Println()

    // 无限循环
    k := 0
    for {
        if k >= 5 {
            break
        }
        fmt.Print(k, " ")
        k++
    }
    fmt.Println()

    // 嵌套循环
    for i := 0; i < 3; i++ {
        for j := 0; j < 2; j++ {
            if i == 1 && j == 1 {
                continue
            }
            fmt.Printf("(%d, %d) ", i, j)
        }
    }
    fmt.Println()
}
```

#### 3. 函数特性
- **多返回值**：函数可返回多个值，需显式接收。
```go
package main

import "fmt"

func swap(a, b int) (int, int) {
    return b, a
}

func main() {
    x, y := 10, 20
    x, y = swap(x, y)
    fmt.Println(x, y)
}
```
- **闭包**：匿名函数可捕获外部变量，形成闭包。
```go
package main

import "fmt"

func getSequence() func() int {
    i := 0
    return func() int {
        i++
        return i
    }
}

func main() {
    nextNumber := getSequence()
    fmt.Println(nextNumber())
    fmt.Println(nextNumber())
    fmt.Println(nextNumber())

    newNextNumber := getSequence()
    fmt.Println(newNextNumber())
}
```
- **延迟执行**：`defer` 注册延迟调用，按栈顺序执行（后进先出），常用于资源释放。
```go
package main

import "fmt"

func main() {
    defer fmt.Println("Last")
    defer fmt.Println("Second")
    fmt.Println("First")
}
```

### 二、并发模型（核心优势）
#### 1. Goroutine
- 轻量级协程，由 Go 运行时调度，开销极小（可创建百万级），比线程高效。
- 启动方式：`go func()`
```go
package main

import (
    "fmt"
    "time"
)

func printNumbers() {
    for i := 0; i < 5; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Print(i, " ")
    }
}

func main() {
    go printNumbers()
    time.Sleep(1 * time.Second)
    fmt.Println("Main goroutine finished")
}
```

#### 2. Channel
- 协程间通信管道，分无缓冲（同步阻塞）和有缓冲（异步队列）。
```go
package main

import (
    "fmt"
)

func main() {
    // 无缓冲通道
    ch := make(chan int)
    go func() {
        ch <- 10
    }()
    num := <-ch
    fmt.Println(num)

    // 有缓冲通道
    bufferedCh := make(chan int, 2)
    bufferedCh <- 20
    bufferedCh <- 30
    fmt.Println(<-bufferedCh)
    fmt.Println(<-bufferedCh)
}
```

#### 3. 同步原语
- **互斥锁**：`sync.Mutex` 保护临界区。
```go
package main

import (
    "fmt"
    "sync"
)

var (
    counter int
    mutex   sync.Mutex
)

func increment() {
    mutex.Lock()
    defer mutex.Unlock()
    counter++
}

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 1000; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            increment()
        }()
    }
    wg.Wait()
    fmt.Println(counter)
}
```
- **读写锁**：`sync.RWMutex` 允许多读单写。
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

var (
    data     int
    rwMutex  sync.RWMutex
)

func readData() {
    rwMutex.RLock()
    defer rwMutex.RUnlock()
    fmt.Println("Read data:", data)
    time.Sleep(100 * time.Millisecond)
}

func writeData() {
    rwMutex.Lock()
    defer rwMutex.Unlock()
    data++
    fmt.Println("Write data:", data)
    time.Sleep(200 * time.Millisecond)
}

func main() {
    var wg sync.WaitGroup

    // 启动多个读操作
    for i := 0; i < 3; i++ {
        wg.Add(1)
        go func() {
            defer wg.Done()
            readData()
        }()
    }

    // 启动一个写操作
    wg.Add(1)
    go func() {
        defer wg.Done()
        writeData()
    }()

    wg.Wait()
}
```
- **WaitGroup**：`sync.WaitGroup` 等待一组协程结束。
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("Worker %d starting\n", id)
    time.Sleep(time.Second)
    fmt.Printf("Worker %d done\n", id)
}

func main() {
    var wg sync.WaitGroup
    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go worker(i, &wg)
    }
    wg.Wait()
    fmt.Println("All workers finished")
}
```

### 三、数据结构
#### 1. 切片（Slice）
- 动态数组，基于数组封装，含指针、长度、容量三属性。
- 操作：`make()` 创建、`append()` 追加（自动扩容）、`copy()` 深拷贝。
```go
package main

import (
    "fmt"
)

func main() {
    // 创建切片
    slice := make([]int, 0, 5)
    // 追加元素
    slice = append(slice, 1, 2, 3)
    fmt.Println(slice)

    // 深拷贝
    newSlice := make([]int, len(slice))
    copy(newSlice, slice)
    fmt.Println(newSlice)
}
```

#### 2. 映射（Map）
- 键值对集合，无序，使用 `make(map[key]value)` 初始化。
- 安全访问：需配合 `sync.Map` 或互斥锁应对并发。
```go
package main

import (
    "fmt"
    "sync"
)

func main() {
    // 初始化 Map
    m := make(map[string]int)
    m["one"] = 1
    m["two"] = 2
    fmt.Println(m)

    // 并发安全的 Map
    var syncMap sync.Map
    syncMap.Store("three", 3)
    value, ok := syncMap.Load("three")
    if ok {
        fmt.Println(value)
    }
}
```

#### 3. 结构体（Struct）
- 组合不同类型字段，支持匿名字段和嵌套，实现组合式“继承”。
- 方法定义：`func (s Struct) method() {}`，接收者可为值或指针。
```go
package main

import (
    "fmt"
)

// 定义结构体
type Animal struct {
    Name string
    Age  int
}

// 定义方法
func (a Animal) Speak() {
    fmt.Printf("%s says hello!\n", a.Name)
}

// 嵌套结构体
type Dog struct {
    Animal
    Breed string
}

func main() {
    dog := Dog{
        Animal: Animal{Name: "Buddy", Age: 3},
        Breed:  "Golden Retriever",
    }
    dog.Speak()
    fmt.Println(dog.Breed)
}
```

### 四、面向对象特性
#### 1. 封装
- 通过首字母大小写控制可见性：大写公有，小写私有。
```go
package main

import (
    "fmt"
)

// 公有结构体
type PublicStruct struct {
    PublicField string
    privateField string
}

// 公有方法
func (p PublicStruct) PublicMethod() {
    fmt.Println("Public method called")
}

// 私有方法
func (p PublicStruct) privateMethod() {
    fmt.Println("Private method called")
}

func main() {
    ps := PublicStruct{PublicField: "Hello"}
    ps.PublicMethod()
    // ps.privateMethod() // 无法调用私有方法
}
```

#### 2. 接口（Interface）
- 隐式实现：类型无需显式声明接口，只需实现全部方法（Duck Typing）。
- 空接口 `interface{}` 可接收任意类型（类似 `any`）。
```go
package main

import (
    "fmt"
)

// 定义接口
type Shape interface {
    Area() float64
}

// 定义结构体
type Circle struct {
    Radius float64
}

// 实现接口方法
func (c Circle) Area() float64 {
    return 3.14 * c.Radius * c.Radius
}

func printArea(s Shape) {
    fmt.Println("Area:", s.Area())
}

func main() {
    circle := Circle{Radius: 5}
    printArea(circle)

    // 空接口
    var anyValue interface{}
    anyValue = 10
    fmt.Println(anyValue)
    anyValue = "Hello"
    fmt.Println(anyValue)
}
```

#### 3. 多态
- 接口变量可持有不同实现类型的实例，调用同名方法表现不同行为。
```go
package main

import (
    "fmt"
)

// 定义接口
type Speaker interface {
    Speak()
}

// 定义结构体
type Dog struct{}
type Cat struct{}

// 实现接口方法
func (d Dog) Speak() {
    fmt.Println("Woof!")
}

func (c Cat) Speak() {
    fmt.Println("Meow!")
}

func makeSpeak(s Speaker) {
    s.Speak()
}

func main() {
    dog := Dog{}
    cat := Cat{}

    makeSpeak(dog)
    makeSpeak(cat)
}
```

### 五、高级特性与工具
#### 1. 错误处理
- 多返回值返回 `error` 对象，配合 `if err != nil` 检查。
- 自定义错误：`errors.New()` 或实现 `error` 接口。
```go
package main

import (
    "errors"
    "fmt"
)

func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, errors.New("division by zero")
    }
    return a / b, nil
}

func main() {
    result, err := divide(10, 0)
    if err != nil {
        fmt.Println("Error:", err)
    } else {
        fmt.Println("Result:", result)
    }
}
```

#### 2. 包管理
- **初始化**：`init()` 函数在包导入时自动执行。
- **依赖**：Go Modules 管理版本（`go.mod`）
```go
// main.go
package main

import (
    "fmt"
    "./mypackage"
)

func main() {
    mypackage.PrintMessage()
}

// mypackage/mypackage.go
package mypackage

import "fmt"

func init() {
    fmt.Println("Initializing mypackage")
}

func PrintMessage() {
    fmt.Println("Hello from mypackage")
}
```

#### 3. 性能优化技巧
- **内存复用**：使用 `sync.Pool` 减少对象分配（如 `fasthttp` 复用 RequestCtx）。
```go
package main

import (
    "fmt"
    "sync"
)

type MyObject struct {
    Data []int
}

var objectPool = sync.Pool{
    New: func() interface{} {
        return &MyObject{Data: make([]int, 0, 100)}
    },
}

func main() {
    obj := objectPool.Get().(*MyObject)
    defer objectPool.Put(obj)

    obj.Data = append(obj.Data, 1, 2, 3)
    fmt.Println(obj.Data)
}
```
- **高效数据结构**：优先用 `[]byte` 替代 `string`（避免不可变性带来的内存复制）。
```go
package main

import (
    "fmt"
)

func main() {
    // 使用 []byte
    byteSlice := []byte("Hello")
    byteSlice = append(byteSlice, ' ', 'W', 'o', 'r', 'l', 'd')
    fmt.Println(string(byteSlice))
}
```

#### 4. 常用命令
- `go build`：编译项目
- `go run`：编译并运行
- `go get`：安装依赖
- `go test`：执行测试
- `go mod init`：初始化模块管理

这些命令通常在终端中使用，例如：
```bash
# 初始化模块管理
go mod init myproject
# 安装依赖
go get github.com/some/package
# 编译项目
go build
# 编译并运行
go run main.go
# 执行测试
go test
```
