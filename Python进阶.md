# Python进阶

## 1. 内存管理机制

Python基于C语言实现。基于C语言的结构体来维护对应的对象。

- float继承了PyObject结构体。PyObject结构体中，定义了：

  - 数据类型

  - 两个指针分别指向前后元素，维护了一个双向链表refchain

  - 引用计数器

- int/list/dict/set/tuple/str/bool 继承了PyVarObject结构体。

  - python的int不限制长度，所以底层是由类似字符串的原理实现

  - bool由int组成
  - PyVarObject继承自PyObject，除了PyObject中的三个元素，还包含：
    - 内部元素的个数

在python中创建对象时，如：

```python
v = 0.3
```

在对应的C语言源码内部：

- 开辟内存malloc
- 做数据的初始化
  - 引用计数器refcnt = 1
  - val = 0.3
  - type = float
- 将对象加入双向链表中（初始化两个指针）

操作：```name=v```，v源码内部，引用计数器refcnt += 1

操作：```del v```，引用计数器refcnt -= 1

操作：

````python
def fun(arg):
  return

fun(name)
````

执行函数传参时，`arg=name`引用计数器加一，函数执行完，`del arg`, refcnt -= 1。

操作：`del name`，引用计数器refcnt -= 1。此时refcnt == 0。

每次引用计数器-1时，都会检查是否为0，如果为0，则判定为垃圾，进行回收。

为了节省开辟内存的时间和性能消耗，在refcnt为0后，不会立刻销毁该片内存，而是存入垃圾缓存free_list中。如果有新的同样类型的变量被创建，则直接调用该内存区域，不需要重新开辟。free_list缓存长度最大为100，存储超过100个则直接销毁该内存。

```python
v1 = 1.1 							# 开辟内存，refcnt = 1，type = float，存入双向链表
id(v1)								# 输出地址4307331376
name = v1							# refcnt += 1
id(name)							# 输出地址4307331376
del name							# refcnt -= 1
del v1								# refcnt -= 1，此时为0，且free_list不满，存入free_list
v112233 = 112233.4		# 从free_list拿出float类型的区域，直接赋值，refcnt=1
id(v112233)						# 输出地址4307331376
```

## 2. 垃圾回收机制

引用计数器为主，分代回收和标记清除为辅。

只使用引用计数器，容器类的元素（list，dict等）会存在循环引用。

```python
a = [1, 2]
b = [3, 4]
a.append(b)			# b的引用计数器refcnt += 1，变为2
b.append(a)			# a的引用计数器refcnt += 1，变为2 
del a						# a的引用计数器refcnt -= 1，变为1
del b						# b的引用计数器refcnt -= 1，变为1
```

此时，a和b理应都被销毁，但是由于循环引用，他们的引用计数器并不为0，并不会被回收。如果类似的代码过多，则造成内存泄漏。

为了解决这种问题，引入了标记清除。定期扫描单独存储容器类元素的双向链表，检查循环引用，如果有，则各自的引用计数器减1。如果为0，则回收。

分代回收：为了优化标记清除扫描对象，把那些扫描中没有出现循环引用的对象，放到上一级的链表中。默认情况下，下一级扫描10次，上一级才扫描1次。总共有3代。

所以，总共有4个双向链表，一个维护非容器类的不会出现循环引用的对象，另外三个作为3代，维护容器类的对象。

## 3. 一切皆对象

Python中所有元素都是一个class。

```python
def func():
  """ 函数1的注释 """
  return 123

print(func.__name__)			# 获取函数名称
print(func.__doc__)				# 获取函数中三引号注释
```

## 4. 装饰器

在不改变原函数的情况下，给原函数添加新的功能。

在某个函数的上方，使用`@func_name`，在python内部会自动执行func_name并且把下方的函数作为参数，传递给func_name，并把下方的函数返回。

```python
def outer(origin):
  # 接收原函数
  def inner(*args, **kwargs):
    # 接收原函数传入的参数
    # 需要添加的功能
    res = origin(*args, **kwargs)		# 传入原函数的参数并执行
    # 需要添加的功能
    return res											# 返回原函数的返回值
  # 返回一个新的函数
  return inner

@outer		# 等价于 func = outer(func)
def func(a1, a2, a3):
  value = [a1, a2, a3]
  return value

func()
```

由于此时装饰器实际上是把inner函数赋给了func，所以如果调取`func.__name__`等类内变量时，会导致调取到的变量为inner的变量，而不是原func的。为了解决这个问题，可以引入functools。

```python
import functools

def outer(origin):
  @functools.wraps(func)			# 将原函数类内的各个元素包装给装饰器：inner.__name__ = func.__name__
  def inner(*args, **kwargs):
    print("before")
    res = origin(*args, **kwargs)		
    print("after")
    return res										
  return inner

@outer		
def func(a1, a2, a3):
  value = [a1, a2, a3]
  return value

func()
```

## 5. 正则表达式

引入python中的re模块进行正则匹配

### 1. 字符相关

- 匹配文本中的固定内容的字符串`dingdalao`

  ```python
  import re
  text = "dingdalao niu bi"
  data_list = re.findall("dingdalao", text)
  print(data_list)			# 生成列表["dingdalao"]
  ```

- 提取文本中所有的a或b或c

  ```python
  text = "dingdalao niu bi"
  data_list = re.findall("[abc]", text)
  print(data_list)			# 生成列表['a', 'a', 'b']
  ```
  
  提取文本中所有的da，db，dc
  
  ```python
  text = "dingdalao niu bi"
  data_list = re.findall("d[abc]", text)
  print(data_list)			# 生成列表['da']
  ```
  
- 匹配除了abc以外的字符

  ```python
  text = "dingdalao niu bi"
  data_list = re.findall("[^abc]", text)
  print(data_list)			# 生成列表['d', 'i', 'n', 'g', 'd', 'l', 'o', 'n', 'i', 'u', 'i']
  ```
  
- 匹配范围中的任意字符

  ```python
  text = "ta1tb2tc3"
  data_list = re.findall("t[a-z]", text)
  print(data_list)		# 生成列表['ta', 'tb', 'tc']
  ```
  
- `.`代指除了换行符的任意一个字符（包括空格）

  ```python
  text = "raoroo"
  data_list = refindall("r.o", text)
  print(data_list)		# 生成列表['rao', 'roo']
  ```
  
  贪婪匹配，+代表一次或多次，能匹配长的就匹配长的
  
  ```python
  text = "rao123123roo"
  data_list = refindall("r.+o", text)
  print(data_list)		# 生成列表['rao123123roo']
  ```
  
  非贪婪匹配，匹配最近的最短的
  
  ```python
  text = "rao123123roo"
  data_list = refindall("r.+?o", text)
  print(data_list)		# 生成列表['rao']
  ```
  
- `\w`代表字母（汉字）或者数字或者下划线

  ```python
  text = "123a丁大佬牛逼b"
  data_list = refindall("a\w+b", text)
  print(data_list)		# 生成列表['a丁大佬牛逼b']
  ```

  如果字符串中有空格，则当成多个字符串分别匹配。

  ```python
  text = "abdadc abdc"
  data_list = re.findall("a\w+c")
  print(data_list)		# 生成列表['abdabc', 'abdc']
  ```

- `\d`代表数字

  ```python
  text = "a123a丁大佬牛逼c1231"
  data_list = re.findall("\w\d+\w", text)
  print(data_list)		# 生成列表['a123a', 'c1231']
  ```

- `\s`代表任意空白符，包括空格和制表符

  ```python
  text = "123 234 456 "
  data_list = re.findall("\w+\s\w+", text)
  print(data_list)		# 生成列表['123 234']
  ```

### 2. 数量相关

- `*`重复0次或多次

- `+`重复1次或多次

- `?`重复0次或1次，和`+`后面的问号含义不一样。

- `{n}`重复n次

  ```python
  text = "123234456 "
  data_list = re.findall("123\d{6}", text)
  print(data_list)		# 生成列表['123234456']
  ```

- `{n,}`重复多于n次

  ```python
  text = "123234456 "
  data_list = re.findall("\d{6,}", text)
  print(data_list)		# 生成列表['123234456']
  ```

- `{n, m}`重复n到m次

### 3. 括号（分组）

- 不影响匹配的规则，改变提取数据的区域，根据所有要求去匹配对应的字符串，但是只输出括号内部的匹配的值

  ```python
  text = "123234456 "
  data_list = re.findall("123(\d{6})", text)
  print(data_list)		# 生成列表['234456']
  ```

  多个括号(匹配到的同一字符串内的分组写入一个元组)

  ```python
  text = "123234456 123123123"
  data_list = re.findall("(12)3(\d{6})", text)
  print(data_list)		# 生成列表[('12', '234456'), ('12', '123123')]
  ```

  括号内套括号

  ```python
  text = "123234456 "
  data_list = re.findall("1(23(\d{6}))", text)
  print(data_list)		# 生成列表['23234456', '234456']
  ```

- 提取指定区域+或条件

  ```python
  text = "123234456,12323dingdalaoniubi"
  data_list = re.findall("12323(\d{4}|d\w+u)", text)
  print(data_list)		# 生成列表['4456', 'dingdalaoniu']
  ```

  先提取全部符合的字符，再提取其中某一部分

  ```python
  text = "123234456,12323dingdalaoniubi"
  data_list = re.findall("(12323(\d{4}|d\w+u))", text)
  print(data_list)		# 生成列表['4456', 'dingdalaoniu']# [('123234456', '4456'), ('12323dingdalaoniu', 'dingdalaoniu')]
  ```

### 4. 起始和结束

- 起始符`^`：用户输入必须严格以指定要求开头
- 结束符`$`：用户输入必须严格以指定要求结尾

### 5. 示例

- 匹配qq号，不能由0开头，至少有5位

  ```python
  "[0-9]\d{4,}"
  ```

- 匹配身份证号，共18位，前17位数字，最后一位是数字或者大写的X

  ```python
  "\d{17}[\dX]"
  ```

  匹配身份证号，并提取最后一位

  ```python
  "(\d{17}(\d|X)"
  ```

  提取地区码6位，年4位月2位日2位

  ```python
  "(\d{6})(\d{4})(\d{2})(\d{2})\d{3}[\dX]"
  ```

- 匹配11位手机号，第一位是1，第二位为3到9

  ```python
  "1[3-9]\d{9}"
  ```

- 匹配邮箱xxxx@xx.xxx。点需要转译。

  ```python
  "[a-zA-z0-9_-]+@[a-zA-z0-9_-]+\.[a-zA-z0-9_-]+"
  ```

## 6. 迭代器和生成器

### 迭代器定义：

- 类中定义了`__iter__`和`__next__`方法
- `__iter__`方法返回对象本身self
- `__next__`方法返回下一个数据，如果没有数据了，则抛出stopiteration异常

迭代器类

```python
class IT(objects):
  def __init__(self):
    self.counter = 0
    
  def __iter__(self):
    return self
  
  def __next__(self):
    self.counter += 1
    if self.counter == 3:
      raise StopIteration()
    return self.counter
```

迭代时，有两种方法：

- 使用`IT.__next__()`
- 使用python内部函数`next(IT)`

支持for循环：

```python
obj2 = IT()
for item in obj2:				# 首先或执行迭代器对象__iter__方法并获取返回值，反复执行next(对象)直到终止
  print(item)
```

### 生成器定义：

生成器是一种特殊的迭代器

```python
# 创建生成器函数
def func():
  yield 1
  yield 2
  
obj = func()
for item in obj:
  print(item)
```

yield出现在函数内部时，函数自动转化为生成器对象。yield类似return。它的作用是，当函数执行到yield，会抛出当前的值，然后停在yield这一行。执行next方法后，接着上次停止的地方，继续执行。

```python
def foo():
  while True:
    res = yield 4
    print("res:", res)
    
obj1 = foo()
print(next(obj1))						# 停在yield这一行，返回4
print(obj1.send(7))					# send方法传入一个数据代替刚刚停在的yield部分，并包含next方法，继续执行直到碰到下一个yield。
# 先打印 res: 7，再继续执行，返回4并输出
```

### 可迭代对象：

一个类中有`__inter__`方法并且该方法返回一个迭代器对象，则称这个类为可迭代对象，可以被for循环。

```python
class Foo(object):
  
  def __iter__(self):
    return iteratorObject
  
obj = Foo()
for item in obj:			# 内部执行该对象的__iter__，获取到的返回值是一个迭代器对象，不断执行该迭代器对象的next方法
  print(item)
```

### Python3中的range函数

`range()`创建了一个可迭代对象，而不是直接生成一个列表。代替了python2中的xrange。舍弃了原来直接生成列表的range，因为非常浪费空间。

使用生成器实现range方法

```python
class Xrange(object):
  
  def __init__(self, max_num):
    self.max_num = max_num
   
 	def __iter__(self):
    counter = 0
    while counter < self.max_num:
      yield counter
			counter += 1
      
      
obj = Xrange(100)
for item in obj:
  print(item)
# 输出0到99
```

### Python中的可迭代对象：

- 列表
- 元组
- 字典
- 集合

## 7. 进程和线程

### 线程：

计算机中可被CPU调度的最小单元

### 进程：

计算机中分配资源的最小单元

一个程序，至少有一个进程，一个进程中至少有一个线程，最终是线程在工作。

### 多线程：

使用`threading`模块。创建多个线程，同时下载列表中的文件

```python
import threading
import request
import time

url_list = [xxx, xxx, xxx]			# 下载资源的url列表

def task(filename, url):
  res = request.get(video_url)
  with open(filename, mode="wb") as f:
    f.write(res.content)
  print(time.time())
  
print(time.time())
for name url in url_list:
  # 创建线程，使用的函数为target，传入函数的参数单独写出来为args
  t = threading.Thread(target=task, args=(name, url))
  # 线程开始工作
  t.start()
```

### 多进程：

使用`multiprocessing`模块。创建多个进程，同时下载列表中的文件。使用多进程时，一定要把多进程创建的部分包裹在name==main中，多线程也推荐放入。python底层多于不同操作系统创建进程的不一样。

```python
import multiprocessing
import request
import time

url_list = [xxx, xxx, xxx]			# 下载资源的url列表

def task(filename, url):
  res = request.get(video_url)
  with open(filename, mode="wb") as f:
    f.write(res.content)
  print(time.time())
  
  
if __name__ == "__main__":
print(time.time())
  for name url in url_list:
    # 创建进程，使用的函数为target，传入函数的参数单独写出来为args
    t = multiprocessing.Process(target=task, args=(name, url))
    # 进程开始工作
    t.start()
```

多进程理论上开销更大

### GIL锁

