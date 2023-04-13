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

#### 在python中的float对象：

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

#### python中的list对象：

- 前后指针
- 引用计数器
- 指针的指针（列表中存放的元素）
- 列表中元素个数
- 列表的容量

创建列表时：

```python
name_list = []
```

- 初始化前后指针Ob_next, ob_prev，把列表存入链表

- 引用计数器ob_refcnt = 1

- 此时列表没有元素，ob_size = 0

- 分配的容量 allocated = 0

- ob_item = null

当插入元素时：

```python
name_list.append("123")
```

- 发现此时插入后列表大小，大于分配的容量，则分配更多连续的空间(0， 4，8，16，24，32，40，52，64，76...)。

- allocated = 4，

- obj_size= 1，

- 拿到字符串123的地址，存入分配到的地址的第一个位置。所以，对应的元素ob_item为指针的指针，先取到列表地址，再在列表中取到字符串地址。

继续插入数据，插到第五个时，又放不下了，则扩容至8。用C语言中的realloc方法，先检查原地址后空间够不够，够的话接着创建，不够则开辟新的空间，并把原来存储的数据搬过来。

如果当前存储的元素个数小于列表容量的一半，列表会缩容。

移除尾部元素pop()：

- 给列表最后一个位置的元素当垃圾回收（加入列表时相当于引用计数器+1，此时引用计数器-1），列表的的ob_size -= 1。不需要在列表中删除最后一个元素的地址，只需要将尾部指针前移，新插入元素时就会自动覆盖掉已经清理掉的地址。

- 如果移除元素的位置不在尾部，需要进行位置移动。

清空列表：

- 和pop()尾部元素机制相似，直接把列表大小和分配的容量还有item指针清空，然后对列表每个元素的引用计数器-1。

销毁列表：

- 首先，引用计数器-1。

  - 如果此时引用计数器还>0，则代表还有其他变量使用此列表，不进行任何操作

  - 否则，表示此列表已经没有任何引用，进行清空：
    - 如果列表中有元素，把每个元素的引用计数器-1
    - 再将自己的ob_tiem = NULL, ob_size = 0, ob_allocated = 0
    - 将列表从管理内存的双向环状列表删除。
    - 将分配的地址存入垃圾缓存free_list，默认列表的free_list容量为80。超过80则直接销毁该对象。

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

全局解释器锁。保证一个进程中同一个时刻只有一个线程可以被CPU调度。

- 计算密集型：每个操作都需要用CPU，所以多线程会被GIL锁住，只能用多进程
- IO密集型：等待IO的时间，不需要利用CPU，可以利用多线程

### 多线程开发：

`threading.Thread`实际上创建了一个子线程。主线程继续向下执行，由子线程执行刚刚创建的任务，当所有子线程都运行完毕，主线程才结束。

- `t.start()` 当前创建的子线程准备就绪，等待CPU调度

- `t.join()` 等待当前线程的任务执行完毕后再向下继续执行

- `t.setDaemon(Bool: Bool)` 守护线程，必须放在start前

  - `t.setDaemon(True)` 设置为守护线程，主线程执行完毕后，子线程也自动关闭
  - `t.setDaemon(False)` 设置为非守护线程，主线程等待子线程执行完毕后，主线程才结束（默认）

- 设置线程的名字，必须放在start前

  ```python
  def task(arg):
      # 获取当前的线程，然后获取它的名字
      name = threading.current_thread().getName()
      print(name)
  
  t = threading.Thread(target=task, args=(11,))
  t.setName(f'thread{1}')
  t.start()
  ```

- 自定义线程类，将线程需要做的事写进run方法中

  ```python
  import threading
  
  class MyThread(threading.Thread):
      def run(self):
          print('执行此线程', self._args)
          
  t = MyThread(args=(100, ))
  t.start()
  ```

### 线程安全：

一个进程可以有多个线程，且多个线程共享进程中的资源。

多个线程同时去操作一个数据，可能会导致数据混乱。可以给资源上锁，防止多个线程同时操作。

```python
lock_object = threading.RLock()

number = 0
loop = 100000

def _add(count):
    lock_object.acquires()			# 加锁
    global number
    for i in range(count):
        number += 1
    lock_object.release()			# 释放
```

也可以基于上下文（with），类似文件管理

```python
def _add(count):
    with lock_object:			# 加锁，运行完自动释放
        global number
        for i in range(count):
            number += 1
```

Python中，有些操作是线程安全的，即操作时自动上锁，例如列表的append操作。

![image-20230409014101164](C:\Users\Selene\AppData\Roaming\Typora\typora-user-images\image-20230409014101164.png)

### 线程锁：

有两种常见的锁：

- Lock：同步锁，不支持锁的嵌套。嵌套则出现**死锁**。由于只有一把锁，再第一个位置锁上了，没有解开的时候就上锁第二次，会导致线程卡死在第二次锁的地方。
- RLock：递归锁，支持锁的嵌套

### 死锁：

由于竞争或者由于彼此通信而造成的一种阻塞现象。

- 同步锁嵌套

- 两个线程两把锁锁了两个资源，要互相申请访问另一把锁

  ```python
  L1, L2 = threading.Lock(), threading.Lock()
  def t1:
      L1.acquire()			# 锁住L1
      time.sleep(1)
      L2.acquire()			# 申请L2锁，发现被t2占用
      print(111)
      L2.release()
      L1.release()
      
  def t2:
      L2.acquire()			# 锁住L2
      time.sleep(1)
      L1.acquire()			# 申请L1锁，发现被t1占用
      print(222)
      L1.release()
      L2.release()
  ```

### 线程池：

线程不是开的越多越好，开多了会导致上下文切换耗时。

使用线程池+回调函数：

```python
from concurrent.futures import ThreadPoolExecutor, future

def task(*args):
    print("xxx")
    time.sleep(5)
    return "xxx"

def done(response):
    # 由子线程执行
    print("执行完后的返回值:", response.result())
    
    
# 创建一个大小为10的线程池
pool = ThreadPoolExecutor(10)

for _ in range(1000):
    # 提交一个任务到线程池，如果有空闲线程，则分配一个任务去执行，执行完毕后再交还给线程池。如果没有空闲线程，则等待。
    future = pool.submit(task, *args)
    # 执行完后，由主线程拿到对应任务的返回值
    future.add_done_callback(done)
pool.shutdown(True)			# 主线程等待所有线程池中所有任务执行完再向下运行
```

统一获取回调结果：

```python
from concurrent.futures import ThreadPoolExecutor, future

def task(*args):
    print("xxx")
    time.sleep(5)
    return "xxx"
    
    
# 创建一个大小为10的线程池
pool = ThreadPoolExecutor(10)
# 接收返回值
future_list = []

for _ in range(1000):
    # 提交一个任务到线程池，如果有空闲线程，则分配一个任务去执行，执行完毕后再交还给线程池。如果没有空闲线程，则等待。
    future = pool.submit(task, *args)
    future_list.append(future)
    
pool.shutdown(True)			# 主线程等待所有线程池中所有任务执行完再向下运行

for fu in future_list:
    print(fu.rresult())
```

案例：

通过线程池，下载文件中每一行对应的图片。

```python
import os
import requests
from concurrent.futures import ThreadPoolExecutor

def download(file_name, image_url):
    res = requests.get(
    	url = image_url,
        headers = {
            # 用于反爬
        	"User_Agent": "xxx"
        }
    )
    return  res

# 创建闭包，获取文件名，因为response不含文件名
def outer(file_name)
    def save(response):
        res = response.result()
        # 检查目录是否存在，如果不存在则创建
        if not os.path.exists("images"):
            os.makedirs("images")

        file_path = os.path.join("images", file_name)
        with open(file_path, mode='wb') as img_object:
            img_object.write(res.content)
	return save

pool = ThreadPoolExecutor(10)

with open("xxx.csv", mode="r", encoding="utf-8") as file_object:
    for line in file_object:
        nid, name, url = line.split(",")
        filename = f"{name}.png"
        furture = pool.submit(download, file_name, url)
        future.add_done_callback(outer(filename))
```

### 单例模式：

面向对象+多线程

之前写一个类，每次调用类都会实例化一个类对象。

单例模式则是，每次实例化类对象时，都用最开始创建的那个对象，不再重复创建。

```python
class Singleton(object):
    instance = None			# 如果已经实例化该类，则置为已经实例化的对象
    
    def __init__(self):
        pass
    
    def __new__(cls, *args, **kwargs):
        # 如果为空，则创建对象，否则，返回创建过的对象
        if cls.instance:
            return cls.instance
        cls.instance = super.__new__(cls)
        return cls.instance
```

如果使用多线程创建该对象，则很有可能在第一个对象更改instance值的过程中，时间片用完，被cpu掉走执行另一个任务，最终，部分线程会因为访问到不该为空的instance而新开辟了地址区域。可以通过加锁，解决该问题。

```python
class Singleton(object):
    instance = None			# 如果已经实例化该类，则置为已经实例化的对象
    lock = threading.RLock()
    
    def __init__(self):
        pass
    
    def __new__(cls, *args, **kwargs):
        # 如果已经开辟内存，则不需要加锁直接拿地址，如果没有开辟，需要检查该变量是否被上锁操作。
        if cls.instance:
            return cls.instance
        with cls.Lock:
        # 如果为空，则创建对象，否则，返回创建过的对象
            if cls.instance:
                return cls.instance
            cls.instance = super.__new__(cls)
        return cls.instance
```

### 多进程开发：

进程和进程之间是相互隔离的

python创建进程的三大模式：

- fork：把主进程所有的资源拷贝一份给子进程，unix，支持文件对象/线程锁传参

  ```python
  def task():
      # 相当于深拷贝的主进程的name，这里添加值并不影响主进程的name
      name.append('123')
      print(name)
      
  if __name__ = "__main__":
      multiprocessing.set_start_method("fork")
      name = []
      p1 = multiprocessing.Process(target=task)
      p1.start()
      print(name)			#还是返回空
  ```

- spawn：不拷贝，依赖手动传参来传入必要的数据，windows，unix，不支持文件对象/线程锁等传参

  ```python
  def task():
      # 报错，因为子进程没有拷贝name
      name.append('123')
      print(name)
      
  if __name__ = "__main__":
      multiprocessing.set_start_method("fork")
      name = []
      p1 = multiprocessing.Process(target=task)
      p1.start()
      print(name)	
      
  #################################################################
  def task(data):
      # 手动传入data，也和主进程中的name没有关系
      data.append('123')
      print(data)
      
  if __name__ = "__main__":
      multiprocessing.set_start_method("fork")
      name = []
      p1 = multiprocessing.Process(target=task, args=(name,))
      p1.start()
      print(name)	
  ```

- forkserver：和spawn类似。但是，先创建一个空的模板进程，创建进程的时候，拷贝一边空的模板，然后手动传值。不支持文件对象/线程锁等传参

### 进程常用方法：

- `p.start()` 当前进程准备就绪，等待被CPU调度（实际工作单元是进程中的线程）

- `p.join()` 主进程等待子进程p的任务执行完毕后，再向下继续执行

- `p.daemon` 设置守护进程，必须再start前设置

  - `p.daemon = True` 设置为守护进程，主进程执行完毕后，子进程也自动关闭
  - `p.daemon = False` 设置为非守护进程，主进程等待子进程执行完毕后，主进程才结束（默认）

- 设置进程的名称

  ```python
  import multiprocessing
  
  def task(arg):
      # 获取当前进程并获取它的名字
      print("当前进程名称为:", multiprocessing.current_process().name)
      
  if __name__ = "__main__":
      multiprocessing.set_start_method("spawn")
      p = multiprocessing.Process(target=task, args=("xxx",))
      p.name = "process1"
      p.start()
  ```

- 获取进程id`os.getpid()`，`os.getppid()`

  ```python
  import multiprocessing
  import os
  
  def task(arg):
      # 获取当前进程(getpid)和父进程(getppid)的id
      print(os.getpid(), os.getppid())
      
  if __name__ = "__main__":
      print(os.getpid())
      multiprocessing.set_start_method("spawn")
      p = multiprocessing.Process(target=task, args=("xxx",))
      p.start()
  ```

- 获取进程中的所有线程`threading.enumerate()`

  ```python
  import multiprocessing
  import os
  import threading
  
  def task(arg):
      # 获取当前进程(getpid)和父进程(getppid)的id
      print(threading.enumerate())
      
  if __name__ = "__main__":
      print(os.getpid())
      multiprocessing.set_start_method("spawn")
      p = multiprocessing.Process(target=task, args=("xxx",))
      p.start()
  ```

- 自定义进程类，直接将进程需要做的事写进run方法

  ```python
  class MyProcess(multiprocessing.Process):
      def run(self):
          print("执行此进程", self._args)
          
  if __name__ = "__main__":
      multiprocessing.set_start_method("spawn")
      p = MyProcess(args=("xxx", ))
      p.start()
  ```

- CPU个数 `multiprocessing.cpu_count()`

### 进程间数据共享：

进程间默认是相互隔离的，每个进程都维护自己独立的数据，不共享。

Python有4种方法：

- value和array，由c编写（很少用）

- 利用`Manager()`，创建server进程来管理所有对象，可以被子进程访问

  ```python
  from multiprocessing import Process, Manager
  
  def f(d, 1):
      d[1] = "1"
      d['2'] = 2
      d[0.25] = None
      l.append(666)
  
  if __name__ = "__main__":
      with Manager() as manager:
          d = manager.dict()
          l = manager.list()
          p = Process(target=f, args=(d, l, ))
          p.start()
  ```

- 队列`multiprocessing.Queue()`，先进先出

  ```python
  def task(q):
      for i in range(10):
          q.put(i)
    
  if __name__ = "__main__":
      queue = multiprocessing.Queue()
      p = multiprocessing.Process(target=task, args=(queue,))
      p.start()
      p.join()
      
      print(queue.get())
      print(queue.get())
      print(queue.get())
  ```

- 管道`multiprocessing.Pipe()`，两个单向队列。类似websocket。

  ```python
  def task(conn):
      for i in range(10):
          conn.send(i)
          data = conn.recv()	# 阻塞，等待数据
          print(data)
    
  if __name__ = "__main__":
      parent_conn, child_conn = multiprocessing.Pipe()
      p = multiprocessing.Process(target=task, args=(child_conn,))
      p.start()
      
      info = parent_conn.recv()		# 阻塞，等待数据
      print(info)
      parent_conn.send(666)
  ```

### 进程锁：

多个进程共享同一个资源，需要用到进程锁。`multiprocessing.RLock()`。通过spawn的多进程启动方法，传入lock参数，使所有进程共享一把进程锁。

```python
def task(lock):
    lock.acquire()
    with open("xxx.txt", mode="r", encoding="utf-8") as f:
        current_num = int(f.read())
    lock.release
    
if __name__ = "__main__":
    multiprocessing.set_start_method("spawn")
    lock = multiprocessing.RLock()
    process_list = []
    for i in range(10):
        p = multiprocessing.Process(target=task, args=(lock,))
        p.start()
        process_list.append(p)
    
    # spawn模式中，主进程一定要等所有的子进程执行完
    for item in process_list:
        item.join()
```

### 进程池：

进程过多也会导致效率降低。

```python
import time
from concurrent.futures import ProcessPoolExecutor, ThreadPoolExecutor

def task(num):
    print(num)
    time.sleep(2)
    
if __name__ == "__main__":
    pool = ProcessPoolExecutor(4)
    for i in range(10):
        pool.submit(task, i)
        
    # 等待进程池中所有任务执行完毕后，再继续往后执行
    pool.shutdown(True)
    print("Done")
```

回调函数，调取执行结果并执行指定操作。

```python
import time
from concurrent.futures import ProcessPoolExecutor, ThreadPoolExecutor

def task(num):
    return num
    
def done(response):
    # 由主进程执行
    print(response.result())
    
if __name__ == "__main__":
    pool = ProcessPoolExecutor(4)
    for i in range(10):
        future = pool.submit(task, i)
        futurre.add_done_callback(done)
     
    # 等待进程池中所有任务执行完毕后，再继续往后执行
    pool.shutdown(True)
    print("Done")
```

进程池中要使用进程锁时，要基于Manager中的Lock和RLock来实现。

```python
# lock_object = multiprocessing.RLock()		不能使用
lock_object = manager.RLock()
for i in range(10):
    future = pool.submit(task, lock_object)
```

## 8. 元类

