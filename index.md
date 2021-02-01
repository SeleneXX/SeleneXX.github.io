## 学习笔记
以下代码全部使用python进行编写，相关知识也是python的知识




### 算法笔记
1.[并查集](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/%E5%B9%B6%E6%9F%A5%E9%9B%86.md)

2.[链表逆置](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/%E9%93%BE%E8%A1%A8%E9%80%86%E7%BD%AE.md)

3.[重构二叉树](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/%E9%87%8D%E6%9E%84%E4%BA%8C%E5%8F%89%E6%A0%91.md)

4.[深度优先遍历](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/dfs.md)

5.[迪杰斯特拉算法](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/dijkstra.md)




### leetcode每日一题
1.[找到最小生成树里的关键边和伪关键边](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/1489.md)

2.[青蛙跳台阶](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/%E9%9D%92%E8%9B%99%E8%B7%B3%E5%8F%B0%E9%98%B6%EF%BC%88%E6%96%90%E6%B3%A2%E9%82%A3%E5%A5%91%E6%95%B0%E5%88%97%EF%BC%89.md)

3.[可被5整除的二进制前缀](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/1018.md)

4.[数组形式的整数加法](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/989.md)

5.[水位上升的泳池中游泳](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/778.md)//1631.最小体力消耗路径同理。

6.[保证图可完全遍历](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/1579.md)

7.[最长连续递增序列](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/674.md)

8.[连通网络的操作次数](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/1319.md)

9.[等价多米诺骨牌对应的数量](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/1128.md)

10.[相似字符串组](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/839.md)

11.[公平的糖果棒交换](https://github.com/SeleneXX/SeleneXX.github.io/blob/gh-pages/888.md)




### python内置方法和基础知识
1.lambda匿名函数：可以不需要定义函数体快速简洁的实现一些功能，例如令列表中所有的数都平方：
````python
lambda x: x*x,[y for y in range(10)]
````
同时，也经常和reduce函数一起使用实现简洁的递归,reduce函数需要导入functool库，作用是对参数序列中元素进行累积也就是递归，如求列表元素和，求积同理：
````python
from functools import reduce
list=[1,2,3,4]
print(reduce(lambda x,y: x+y, list))
````

2.python中的 ````**args ````代表传入一个或多个无名的tuple类型的参数，tuple类型是python中不变的数据结构，而````**kwargs````代表传入一个或多个dict类型的参数：
````python
def test(*args):  
    print(args)
    for i in args:
        print(i)

test(1,2,3)
````
其输出为：
(1,2,3)
1
2
3
````python
def test(**kwargs):
    print(kwargs)
    keys = kwargs.keys()
    value = kwargs.values()
    print(keys)
    print(value)

test(a=1,b=2,c=3,d=4)
````
其输出为：
{'a': 1, 'b': 2, 'c': 3, 'd': 4}
dict_keys(['a', 'b', 'c', 'd'])
dict_values([1, 2, 3, 4])

3.Decorator装饰器：本身是一个函数，可以在不改变一个已有函数的代码的情况下拓展一些新的功能。引用时在函数前加上@dec即可。例如：
````python
def dec(f)
    n = 3
    def wrapper(*args, **kw):
        return f(*args, **kw) * n
    return wrapper
@dec
def foo(n):
    return n * 2
foo(2)
````
其中foo(2)等价于：
````python
foo = dec(foo)
foo(2)
````
所以，就等于2 * n * 2，结果为12

4.对列表切片不会导致IndexError。对二维列表切片，若切片位置不存在，返回的是一维空列表[]。

5.re模块实现正则功能，re.match(正则表达式, 字符串, [匹配模式])。正则表达式：````r'(.*)on(.*?).*'````，r代表后面的字符串是普通字符串，\n会被译为\和n而不是换行，（）包裹住的数据为需要提取的数据，通常与group()函数连用。````(.*)````匹配所有字符，````(.*?)````匹配到空格为止。group(1) = 第一个括号，group(2) = 第二个括号，group(0)代表原字符串。

6.python中的序数表示为 real + imag j，其中real和imag都是浮点数。虚数部分必须后缀j或者J。方法conjugate返回共轭复数。

7.运行一个模块时，__name__为__main__，但是在其他模块中import这个模块，__name__为其他模块的名称。例如以下为print_func.py的代码：
````python
print('Hello World!') 
print('__name__ value: ', __name__)   
def main():     
    print('This message is from main function')   
if __name__ == '__main__':     
    main()   
````
以下为print_module.py的代码： 
````python
import print_func 
print("Done!")
````
运行结果为Hello World! __ name__ value: print_func Done!因为是在print_module.py中import print_func.py，所以name不是main，不会执行if语句

8.解码decode，编码encode，urllib为url编码。````urllib.quote(line.decode("gbk").encode("utf-16"))````的含义是，先对line进行gbk解码，然后进行utf16编码，最后进行url编码。所以经过该编码的字符串解码结果正相反，先进行url解码，然后解码utf-16，最后编码gbk

9.python中的三元运算符使用不同于c和java的max = x > y ? x : y，而是max = x if x > y else y。

10.random.random()方法的作用是生成0到1之间的随机浮点数。

