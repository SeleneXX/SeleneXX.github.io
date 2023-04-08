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
  text = "dingdalao niu bi."
  data_list = re.findall("dingdalao", text)
  print(data_list)			# 声称列表["dingdalao"]
  ```

- 提取文本中所有的a或b或c

  ```python
  text = "dingdalao niu bi."
  data_list = re.findall("[abc]", text)
  print(data_list)			# 声称列表['a', 'a', 'b']
  
  ```

  提取文本中所有的da，db，dc

  ```python
  text = "dingdalao niu bi."
  data_list = re.findall("d[abc]", text)
  print(data_list)			# 声称列表['da']
  ```

  



