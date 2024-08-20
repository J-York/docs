# Python 八股文

## return 与 yield 的区别

1. **return**：return 语句用于从函数中返回一个值，并终止函数的执行。当函数执行到 return 语句时，它会立即返回指定的值，并且不再执行函数的其他部分。一个函数可以包含多个 return 语句，但只有其中一个会被执行，因为执行到一个 return 语句后函数就会退出。
2. **yield**：yield 关键字用于定义**生成器函数**，它允许函数**暂停执行，并返回一个中间值**。每次调用生成器函数时，它会从上次暂停的地方继续执行，直到遇到下一个 yield 语句。yield 语句可以多次出现在一个生成器函数中，每次调用生成器函数时，它会执行到下一个 yield 语句，然后暂停，直到再次被调用。生成器函数的执行状态会被保存，这使得它可以生成一个序列而不需要一次性将所有值存储在内存中。

```python
def read_file_return(file_path):
    lines = []
    with open(file_path, 'r') as file:
        for line in file:
            lines.append(line.strip())
    return lines

# 假设我们有一个大文件 'large_file.txt'
all_lines = read_file_return('large_file.txt')
for line in all_lines:
    print(line)
```

在这个示例中，`read_file_return` 函数读取整个文件的内容，并将所有行存储在一个列表中，然后使用 `return` 返回这个列表。这种方式在文件很大时可能会占用大量内存。

```python
def read_file_yield(file_path):
    with open(file_path, 'r') as file:
        for line in file:
            yield line.strip()

# 假设我们有一个大文件 'large_file.txt'
gen = read_file_yield('large_file.txt')
for line in gen:
    print(line)
```

在这个示例中，`read_file_yield` 函数使用 `yield` 逐行读取文件内容，并在每次读取一行时暂停函数的执行。这种方式在处理大文件时更加高效，因为它不需要一次性将所有内容加载到内存中。

```python
def generate_numbers():
    for i in range(5):
        yield i

numbers = generate_numbers()
for num in numbers:
    print(num)
#输出: 0 1 2 3 4
```

**生成器函数**：生成器函数是一种特殊的函数，它可以通过 `yield` 关键字来生成一系列值，而不是一次性返回所有结果。生成器函数可以在需要时生成值，这样可以节省内存，并允许以一种惰性的方式产生数据。在上文中`numbers = generate_numbers()` 只运行了一次，但它创建了一个生成器对象 `numbers。`当你使用 `for num in numbers:` 循环迭代生成器对象时，每次迭代都会触发生成器函数的执行，从上次暂停的地方继续执行，直到遇到下一个 `yield` 语句暂停。这样就会产生多个值，每次迭代都会生成一个新的值。

## 什么是闭包 closure

闭包（Closure）是一种编程语言的特性，指的是一个函数（或者称为内部函数）能够访问其外部函数中的变量，并将该函数及其相关的环境打包成一个整体，形成一个闭包。闭包**允许函数“捕获”并使用其定义时所在环境中的变量**。

在 Python 中，闭包通常是通过在一个函数内部定义另一个函数来实现的。内部函数可以访问外部函数的局部变量，即使外部函数已经执行完毕。

```python
# 使用闭包实现日志记录器
import datetime

def create_logger(log_level):
    def logger(message):
        timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        print(f"[{timestamp}] [{log_level}] {message}")
    return logger

# 创建不同级别的日志记录器
info_logger = create_logger("INFO")
warning_logger = create_logger("WARNING")
error_logger = create_logger("ERROR")

# 使用日志记录器记录消息
info_logger("This is an info message.")
warning_logger("This is a warning message.")
error_logger("This is an error message.")
```

**保持状态**：闭包可以在函数调用之间保持状态，而不需要使用全局变量。

**封装性**：闭包可以封装私有数据和操作，并将其作为单个单元暴露给外部。

**代码复用**：可以利用闭包来封装通用的功能，并在不同的上下文中复用。

**回调函数**：闭包常用于创建回调函数，在事件发生时执行特定的逻辑。

需要注意的是，闭包可能导致**内存泄漏**，因为闭包中包含了对外部作用域的引用，使得外部作用域中的变量无法被垃圾回收。因此，当不再需要闭包时，最好手动解除对其的引用，以释放资源。

## 什么是装饰器

装饰器（Decorator）是 Python 中一种非常强大且灵活的功能，用于修改或增强函数或方法的行为，而无需修改它们的源代码。装饰器**本质上是一个函数，它接受一个函数作为参数，并返回一个新的函数**。

```python
def decorator_function(original_function):
    def wrapper_function(*args, **kwargs):
        # 在调用原始函数之前可以执行一些操作
        print(f"Before calling {original_function.__name__}")
        result = original_function(*args, ** kwargs)
        # 在调用原始函数之后可以执行一些操作
        print(f"After calling {original_function.__name__}")
        return result
    return wrapper_function

@decorator_function
def my_function():
    print("Inside my_function")

my_function()
```

> 闭包是装饰器实现的基础，而装饰器则是闭包的一种应用场景。装饰器利用了闭包的特性来实现对函数的封装和扩展。

## 什么是类方法、实例方法和静态方法

### 类方法

类方法是与类相关联的方法，而不是与实例相关联。通过装饰器 `@classmethod` 来声明，第一个参数通常是 cls，代表类本身。类方法可以访问类的属性和方法，但不能直接访问实例的属性和方法。

> 类变量是在类定义内部但在任何方法之外定义的变量。
> 如果通过实例修改类变量，Python 会创建一个同名的实例变量，而不是修改类变量。

### 实例方法

实例方法是与类的实例相关联的方法。在方法定义中，第一个参数通常是 `self`，代表实例本身。实例方法可以访问实例的属性和方法，也可以访问类的属性和方法。实例方法通常用于实例的初始化、操作和处理实例的状态。

### 静态方法

静态方法与类或实例无关，与普通函数类似，但是定义在类的内部。通过装饰器 `@staticmethod` 来声明，没有默认的第一个参数。静态方法不能访问类的属性和方法，也不能访问实例的属性和方法。静态方法通常用于在类的范围内提供一些功能，这些功能与特定的实例或类无关。

## *arg 和 **kwargs 的区别是什么

### 位置参数与关键词参数

在 Python 中，函数参数可以分为**位置参数（Positional Arguments）**和**关键字参数（Keyword Arguments）**。

位置参数是指在函数调用时**按照参数定义的顺序传递**的参数。位置参数的值是根据它们在函数调用中的位置来确定的。

关键字参数是指在函数调用时**使用参数名来指定参数值**的参数。关键字参数允许我们在调用函数时明确指定参数名，从而不必遵循参数定义的顺序。

在函数调用中，我们可以混合使用位置参数和关键字参数，但有一个重要的规则：**位置参数必须出现在关键字参数之前**。

```python
def add(a, b, c):
    return a + b + c

result = add(3, b=5, c=7)  # 混合使用位置参数和关键字参数
print(result)  # 输出: 15
```

### *arg 与 **kwargs

`*args` 和 `**kwargs` 是 Python 中用于处理函数参数的特殊语法，它们用于接收不定数量的参数，其中 `*args` 用于接收位置参数，而 `**kwargs` 用于接收关键字参数。

**args：**args 是一个元组（tuple），用于接收不定数量的位置参数。

- 当你不确定函数会接收多少个位置参数时，可以使用 *args 来接收。
- 在函数定义时，将 *args 放在参数列表中，表示该函数可以接收任意数量的位置参数。
- 在函数调用时，可以传递任意数量的位置参数给 *args，它们会被自动打包成一个元组传递给函数。

**kwargs：**kwargs 是一个字典（dictionary），用于接收不定数量的关键字参数。

- 当你不确定函数会接收多少个关键字参数时，可以使用 **kwargs 来接收。
- 在函数定义时，将 **kwargs 放在参数列表中，表示该函数可以接收任意数量的关键字参数。
- 在函数调用时，可以传递任意数量的关键字参数给 **kwargs，它们会被自动打包成一个字典传递给函数，其中键是参数名，值是对应的参数值。

## Python 的内存管理方式

Python的内存管理机制涉及多个方面，包括对象的分配和释放、垃圾回收机制、内存池管理等。

### 内存分配和释放

Python使用动态内存分配来管理对象的内存，这意味着内存的分配和释放都是在运行时进行的。Python通过一个名为`PyObject`的结构体来管理对象的引用计数，以此来追踪对象的使用情况。

### 引用计数

Python **使用引用计数作为其主要的内存管理机制**。每个对象都有一个引用计数，当对象被引用时，引用计数增加；当引用被删除时，引用计数减少。当引用计数为零时，对象的内存被自动回收。

### 垃圾回收

虽然引用计数是主要的内存管理机制，但它不能处理**循环引用**的情况。为了解决这个问题，Python 引入了垃圾回收（Garbage Collection）机制。垃圾回收器会定期检查并回收那些存在循环引用的对象。

Python的垃圾回收机制使用了**分代回收**算法，将对象分为三代（generation），新创建的对象属于第0代，存活时间较长的对象会逐渐移动到第1代和第2代。垃圾回收器会优先检查第0代对象，逐渐向更高代次推进。

```py3
import gc
gc.collect()  # 手动触发垃圾回收
```

### 内存池

为了提高内存分配和释放的效率，Python使用了一种称为内存池（Memory Pool）的技术。内存池将小对象（小于256字节）的内存分配委托给一个名为`pymalloc`的专用分配器，而不是直接使用操作系统提供的内存分配函数（如`malloc`）。这种方式减少了内存碎片，提高了内存分配和释放的效率。

### 内存泄漏检测

尽管Python的内存管理机制较为完善，但在某些情况下仍可能发生内存泄漏，特别是在使用外部扩展模块时。为了检测内存泄漏，可以使用Python标准库中的`gc`模块和`tracemalloc`模块。

> Python通过引用计数和垃圾回收机制实现了自动内存管理，并通过内存池技术优化了小对象的内存分配和释放。这些机制共同作用，使得开发者能够专注于业务逻辑，而不必过多担心内存管理的细节

## Python 中的基础数据结构，他们的基本操作和区别

### List

1. **特点** ：有序、可变、允许重复元素。
2. **操作**：
   - 添加元素：`append()`, `insert()`, `extend()`
   - 删除元素：`remove()`, `pop()`, `clear()`
     - `remove()` 移除第一个匹配的元素 O(n)
     - `pop()` 根据索引删除元素，返回被删除的元素 O(n) 或者 O(1)
     - `clear()` 清空列表中的所有元素
   - 访问元素：通过索引访问，如 `list[0]`
   - 切片：`list[start:end]`
   - 排序：`sort()`, `reverse()`

### Tuple

- **特点** ：有序、不可变、允许重复元素。
- 操作：
  - 访问元素：通过索引访问，如 `tuple[0]`
  - 切片：`tuple[start:end]`
  - 不可变，因此不能添加、删除或修改元素

### Set

- **特点** ：无序、可变、不允许重复元素。（集合使用哈希表来存储元素）
- **操作**：
  - 添加元素：`add()`, `update()`
    - `update()` 将一个可迭代对象的所有元素添加到集合中
  - 删除元素：`remove()`, `discard()`, `pop()`, `clear()`
    - `discard()` 方法与 `remove()` 类似，但它不会在元素不存在时引发错误。这个方法的时间复杂度也是 O(1)。
    - `pop()` 方法从集合中随机删除一个元素并返回它。由于集合是无序的，所以删除的元素是随机的。
  - 集合运算：`union()`, `intersection()`, `difference()`, `symmetric_difference()`
    - **union()** ：返回两个集合的并集。
    - **intersection()** ：返回两个集合的交集。
    - **difference()** ：返回两个集合的差集。
    - **symmetric_difference()** ：返回两个集合的对称差集。
    - **原理** ：这些集合运算方法基于集合的数学定义，使用哈希表来高效地进行操作。它们的时间复杂度通常是 O(n)，其中 n 是集合中元素的数量。

### Dictionary

- 特点：无序、可变、键值对结构，键唯一。

- 操作
  - 添加/修改元素：`dict[key] = value`
  - 删除元素：`del dict[key]`, `pop(key, default)`, `clear()`
  - 访问元素：通过键访问，如 `dict[key]`
  - 遍历：`keys()`, `values()`, `items()`
    - 注意：`keys()` 和 `values()` 返回的是一个视图对象，这两个视图对象是动态的

