---
title: "Python学习笔记"
date: 2025-06-15  # 可选，手动指定日期
layout: default      # 可选，指定布局（如 `post` 或 `default`）
categories: [Python]  # 可选，添加分类
---
# Python 学习笔记

## 一、Python 基础入门

### 1. Python 简介
- **创始人**：吉多·范罗苏姆（Guido von Rossum）。
- **特点**：简洁易读、开发效率高、跨平台、开源且生态圈丰富。
- **应用领域**：大模型、计算机视觉、智能推荐、自动驾驶、数据科学等。

### 2. 发展历程
- 1989 年圣诞，吉多为打发时间开始开发 Python。
- 1991 年发布首个版本 0.9.0。
- 2008 年 Python 3.0 发布，不兼容 Python 2，2020 年停止对 Python 2 的维护。

### 3. 优缺点
- **优点**：简单易学、代码量少、胶水语言（可整合其他语言模块）、跨平台。
- **缺点**：执行效率相对较低（解释型语言特性）。

### 4. 环境安装
- **官方解释器**：从 [Python 官网](https://www.python.org/downloads/) 下载对应系统的安装包。
- **Windows 安装**：安装时勾选“Add Python to PATH”，自定义安装路径（不含中文和特殊字符）。
- **macOS 安装**：下载 pkg 文件，双击安装，通过 `python3 --version` 检查安装结果。
- **包管理工具**：pip，可通过 `pip --version` 检查是否可用。

## 二、第一个 Python 程序

### 1. 编写工具
- **交互式环境**：命令行输入 `python`（Windows）或 `python3`（macOS），适合简单测试。
- **IPython**：增强版交互式环境，需通过 `pip install ipython` 安装。
- **PyCharm**：专业集成开发环境（IDE），适合项目开发，支持代码补全、调试等功能。
- **VS Code**：轻量级编辑器，安装 Python 插件后可用。

### 2. 第一个程序：Hello World
```python
print('hello, world')
```
- `print()` 是内置函数，用于输出内容。
- 字符串可用单引号或双引号包裹，需使用英文符号。

### 3. 注释
- **单行注释**：以 `#` 开头。
- **多行注释**：用三个双引号 `"""` 包裹。

## 三、变量与数据类型

### 1. 变量
- **定义**：用于存储数据的内存空间，可修改和读取。
- **命名规则**：
  - 由字母、数字、下划线组成，数字不能开头。
  - 区分大小写（如 `a` 和 `A` 是不同变量）。
  - 不能与关键字（如 `if`、`for`）和保留字（如 `int`、`print`）重名。
- **命名惯例**：小写字母+下划线（如 `user_name`）。

### 2. 数据类型
- **整型（int）**：如 `100`、`0b100`（二进制）、`0x100`（十六进制）。
- **浮点型（float）**：如 `3.14`、`1.23e2`（科学计数法，等于 123）。
- **字符串（str）**：如 `'hello'`、`"world"`。
- **布尔型（bool）**：值为 `True` 或 `False`。

### 3. 类型转换
- `int()`：转整数，如 `int('123')` → 123。
- `float()`：转浮点数，如 `float('3.14')` → 3.14。
- `str()`：转字符串，如 `str(100)` → `'100'`。
- `chr()`：ASCII 码转字符，如 `chr(97)` → `'a'`。
- `ord()`：字符转 ASCII 码，如 `ord('a')` → 97。

## 四、运算符与表达式

### 1. 算术运算符
- `+`（加）、`-`（减）、`*`（乘）、`/`（除）、`//`（整除）、`%`（取模）、`**`（幂运算）。
- 示例：
```python
print(10 // 3)  # 3
print(10 % 3)   # 1
print(2 ** 3)   # 8
```

### 2. 赋值运算符
- 基本赋值：`=`，如 `a = 10`。
- 复合赋值：`+=`、`-=`、`*=` 等，如 `a += 5` 等价于 `a = a + 5`。
- 海象运算符（Python 3.8+）：`:=`，可在表达式中赋值，如 `print((a := 10))` → 10。

### 3. 比较与逻辑运算符
- **比较运算符**：`==`（等于）、`!=`（不等于）、`<`（小于）、`>`（大于）等，结果为布尔值。
- **逻辑运算符**：`and`（与）、`or`（或）、`not`（非）。
- 示例：
```python
a, b = 5, 10
print(a < b and not b > 20)  # True
```

### 4. 应用示例
- **华氏温度转摄氏温度**：
```python
f = float(input('请输入华氏温度: '))
c = (f - 32) / 1.8
print(f'{f:.1f}华氏度 = {c:.1f}摄氏度')
```
- **判断闰年**：
```python
year = int(input('请输入年份: '))
is_leap = (year % 4 == 0 and year % 100 != 0) or year % 400 == 0
print(f'{year}是闰年' if is_leap else f'{year}不是闰年')
```

## 五、分支结构

### 1. if-else 结构
- 单分支：`if 条件: 语句`。
- 双分支：`if 条件: 语句1 else: 语句2`。
- 多分支：`if 条件1: 语句1 elif 条件2: 语句2 else: 语句3`。
- 示例（BMI 计算）：
```python
height = float(input('身高(cm)：')) / 100
weight = float(input('体重(kg)：'))
bmi = weight / (height ** 2)
if bmi < 18.5:
    print('体重过轻')
elif bmi < 24:
    print('身材标准')
elif bmi < 27:
    print('体重过重')
else:
    print('肥胖')
```

### 2. match-case 结构（Python 3.10+）
- 用于多条件分支，更简洁。
- 示例：
```python
status_code = int(input('响应状态码: '))
match status_code:
    case 404:
        print('Not Found')
    case 403:
        print('Forbidden')
    case _:
        print('Unknown Status Code')
```

## 六、循环结构

### 1. for-in 循环
- 适用于已知次数的循环，可遍历序列（列表、字符串等）。
- 示例（1-100 偶数求和）：
```python
total = 0
for i in range(2, 101, 2):
    total += i
print(total)  # 2550
```
- `range()` 用法：
  - `range(n)`：0 到 n-1 的整数。
  - `range(a, b)`：a 到 b-1 的整数。
  - `range(a, b, step)`：a 到 b-1，步长为 step。

### 2. while 循环
- 适用于条件满足时循环，不确定次数。
- 示例（猜数字游戏）：
```python
import random
answer = random.randint(1, 100)
count = 0
while True:
    count += 1
    num = int(input('请猜数字: '))
    if num < answer:
        print('大一点')
    elif num > answer:
        print('小一点')
    else:
        print(f'猜对了！共猜了{count}次')
        break
```

### 3. 循环控制
- **break**：跳出当前循环。
- **continue**：跳过本次循环剩余代码，进入下一次循环。
- **else**：循环正常结束后执行（未被 break 中断）。

### 4. 嵌套循环
- 示例（打印九九乘法表）：
```python
for i in range(1, 10):
    for j in range(1, i+1):
        print(f'{i}×{j}={i*j}', end='\t')
    print()
```

## 七、数据结构

### 1. 列表（list）
- **特点**：有序、可变、可重复、元素类型任意。
- **创建**：`[1, 2, 3]` 或 `list(iterable)`。
- **常用操作**：
  - 索引：`list[0]`（正向）、`list[-1]`（反向）。
  - 切片：`list[1:3]`（左闭右开）、`list[::2]`（步长 2）。
  - 增删改：`append()`、`insert()`、`remove()`、`pop()`、`list[0] = 10`。
  - 其他：`sort()`（排序）、`reverse()`（反转）、`len(list)`（长度）。
- **列表生成式**：
```python
# 生成 1-10 的平方列表
squares = [x ** 2 for x in range(1, 11)]
# 筛选偶数并平方
even_squares = [x ** 2 for x in range(1, 11) if x % 2 == 0]
```

### 2. 元组（tuple）
- **特点**：有序、不可变、可重复，适合存储不可修改的数据。
- **创建**：`(1, 2, 3)` 或 `tuple(iterable)`，单元素元组需加逗号：`(1,)`。
- **解包**：`a, b = (1, 2)`，可配合星号表达式：`a, *b = (1, 2, 3, 4)` → `a=1, b=[2,3,4]`。

### 3. 字符串（str）
- **特点**：有序、不可变，由字符组成。
- **常用操作**：
  - 索引/切片：与列表类似。
  - 拼接：`'hello' + 'world'`。
  - 格式化：
    - 传统方式：`'{} {}'.format('hello', 'world')`。
    - f-string：`f'hello {world}'`。
  - 方法：`strip()`（去首尾空格）、`split()`（分割）、`replace()`（替换）、`upper()`（转大写）等。

### 4. 集合（set）
- **特点**：无序、唯一、可变，元素需为不可变类型。
- **创建**：`{1, 2, 3}` 或 `set(iterable)`，注意空集合用 `set()`。
- **常用操作**：
  - 增删：`add()`、`remove()`、`pop()`。
  - 运算：`&`（交集）、`|`（并集）、`-`（差集）、`^`（对称差）。

### 5. 字典（dict）
- **特点**：键值对存储，键唯一且为不可变类型，值任意。
- **创建**：`{'name': '张三', 'age': 18}` 或 `dict(key=value)`。
- **常用操作**：
  - 访问：`dict['name']`，存在则报错，建议用 `dict.get('name')`。
  - 增删改：`dict['name'] = '李四'`、`del dict['age']`、`dict.pop('name')`。
  - 遍历：`for key in dict`（遍历键）、`for value in dict.values()`（遍历值）、`for key, value in dict.items()`（遍历键值对）。

## 八、函数与模块

### 1. 函数定义与调用
- **定义**：
```python
def add(a, b):
    """计算两数之和"""
    return a + b
```
- **调用**：`result = add(5, 3)`。
- **参数**：
  - 位置参数：按顺序传参。
  - 关键字参数：`add(b=3, a=5)`。
  - 默认参数：`def add(a, b=0):`。
  - 可变参数：`*args`（元组）、`**kwargs`（字典）。

### 2. 高阶函数
- **函数作为参数**：如 `sorted(list, key=len)`。
- **lambda 函数**：匿名函数，如 `lambda x: x + 1`。
- **偏函数**：固定部分参数，如 `int2 = partial(int, base=2)`。

### 3. 模块与导入
- **导入模块**：
  - `import math`：通过 `math.pi` 访问。
  - `from math import pi`：直接用 `pi`。
  - `from math import *`：导入所有内容（不推荐）。
- **常用模块**：`random`（随机数）、`time`（时间）、`os`（操作系统）、`re`（正则表达式）。

## 九、面向对象编程

### 1. 类与对象
- **类定义**：
```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def study(self, course):
        print(f'{self.name}在学习{course}')
```
- **对象创建**：`stu = Student('张三', 18)`。
- **方法调用**：`stu.study('Python')`。

### 2. 封装、继承、多态
- **封装**：隐藏内部细节，提供公共接口。
- **继承**：
```python
class Graduate(Student):
    def research(self):
        print(f'{self.name}在做研究')
```
- **多态**：不同子类对同一方法有不同实现，如不同形状的 `area()` 方法。

### 3. 特殊方法
- `__init__`：初始化方法。
- `__str__`：定义对象打印时的显示内容。
- `__lt__`：定义 `<` 运算符行为。
- `__len__`：定义 `len()` 函数行为。

## 十、实战应用

### 1. 生成随机验证码
```python
import random
import string

def generate_code(code_len=4):
    """生成指定长度的验证码，包含数字和字母"""
    all_chars = string.digits + string.ascii_letters
    return ''.join(random.choices(all_chars, k=code_len))

print(generate_code())  # 如 'a1B3'
```

### 2. 工资结算系统
```python
from abc import ABCMeta, abstractmethod

class Employee(metaclass=ABCMeta):
    def __init__(self, name):
        self.name = name
    
    @abstractmethod
    def get_salary(self):
        pass

class Manager(Employee):
    def get_salary(self):
        return 15000.0

class Programmer(Employee):
    def __init__(self, name, working_hour=0):
        super().__init__(name)
        self.working_hour = working_hour
    
    def get_salary(self):
        return 200 * self.working_hour

class Salesman(Employee):
    def __init__(self, name, sales=0):
        super().__init__(name)
        self.sales = sales
    
    def get_salary(self):
        return 1800 + self.sales * 0.05

# 使用示例
emps = [Manager('张三'), Programmer('李四'), Salesman('王五')]
for emp in emps:
    if isinstance(emp, Programmer):
        emp.working_hour = int(input(f'输入{emp.name}工作时间: '))
    elif isinstance(emp, Salesman):
        emp.sales = float(input(f'输入{emp.name}销售额: '))
    print(f'{emp.name}月薪: ￥{emp.get_salary():.2f}')
```

