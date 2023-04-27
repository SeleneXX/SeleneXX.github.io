# C++

## 1. 面向对象

```c++
#include <iostream>
#include <string>
using namespace std;

class Wood{
protected:
    string name;
    int weight;
public:
    Wood(string n, int w){
        name = n;
        weight = w;
    }
    void show(){
        cout << "这是一块" << weight << "kg的" << name << endl;
    }
};

int main(){
    Wood w1("黄花梨", 10), w2("金丝楠", 5), w3("沉香木"， 6);
    w1.show();
    w2.show();
    w3.show();
}
```

C++中，类中不加public默认为private。private只有自己能访问，protected可以被子类访问。

### 继承

桌子可以继承自木头

```c++
class Desk: public Wood{
public:
    Desk(string n, int w): Wood(n, w){}
};

int main(){
    Desk d1("黄花梨", 10), d2("金丝楠", 5), d3("沉香木"， 6);
}
```

但是通过继承的函数，打印出来都是木头。

### 多态

使用虚函数

```c++
class Wood{
protected:
    string name;
    int weight;
public:
    Wood(string n, int w){
        name = n;
        weight = w;
    }
    virtual void show(){
        cout << "这是一块" << weight << "kg的" << name << endl;
    }
};
```

在子类中重写该函数

```c++
class Desk: public Wood{
public:
    Desk(string n, int w): Wood(n, w){}
    void show(){
        cout << "这是一块" << weight << "kg的" << name << "做出来的书桌" << endl;
    }
};
```

通过虚函数，可以实现同样的类型指针，指向的对象不同，获得不同的结果。

```c++
void test(Wood* p);

int main(){
    Wood w1("黄花梨", 10);
    Desk d1("金丝楠", 5);
    Wood* p;
    p = &w1;
    test(p);
    p = &d1;
    test(p);
}

void test(Wood* p){
    p->show();
}
```

### 多态的实现

在C++中，类内的变量和函数是分开存储的。函数单独存储在代码区，可以通过作用域取对应的函数`Wood::show()`。如果一个基类中定义了虚函数，这个类的变量存储部分，会多出来4个字节，用于存储虚函数表。虚函数表是一个列表，按照虚函数顺序序号依次存入对应的函数在代码区中的地址。如果有一个子类继承了该基类，则同样也会继承虚函数表。如果子类没有重写某个虚函数，则还是指向原来的函数，如果重写了，则修改对应位置的地址，指向新的虚函数。

| 基类A的虚函数表（&func） | 代码区对应的函数            |
| ------------------------ | --------------------------- |
| 0x441802                 | virtual Func1    (A::Func1) |
| 0x441803                 | virtual Func2    (A::Func2) |

子类继承A

| 子类B的虚函数表（&func） | 代码区对应的函数            |
| ------------------------ | --------------------------- |
| 0x441802                 | virtual Func1    (A::Func1) |
| 0x441803                 | virtual Func2    (A::Func2) |

此时，重写子类B的Func1

| 子类B的虚函数表（&func） | 代码区对应的函数            |
| ------------------------ | --------------------------- |
| 0x441802                 | virtual Func1    (A::Func1) |
| 0x611d09                 | virtual Func2    (B::Func2) |

类指针访问虚函数时，去虚函数表中找，而不是直接去代码区找。第一个虚函数由于没有重写，指针指向B的时候，还是调用基类A的Func1，第二个虚函数被重写了，则修改子类B的虚函数表中对应位置的地址，使其指向重写后的B中的Func2。

可以通过父类指针调用子类。

```c++
// 定义函数指针类型FUNC
typedef void(*FUNC)();
int main(){
    Desk d1("金丝楠", 5);
    // 此时，pTable的值和&d1一样，都是d1的首地址
    // 但是pTable为int类指针，指针调用时，只取前四个字节的数据
    // 所以*pTable取到的值，就是虚函数表的地址
    int* pTable = (int*)&d1;
    // 虚函数表中，存着虚函数的地址，每个虚函数地址也占四个字节
    // 再次新建int指针pFirstFunc，指向虚函数表首地址
    // 该指针取到的值为第一个虚函数的地址
    int* pFirst = (int*)*pTable;
    // 定义函数类型指针，指向该地址
    FUNC pFunc = (FUNC)*pFirst;
    // 函数指针可以不解引用，直接调用函数
    pFunc();
}
```

### 虚函数总结

通过在基类定义虚函数，可以在类中添加一个4字节区域存储虚函数表。所有子类继承父类的同时，继承该组函数表。如果重写某个虚函数，则会在虚函数表中重写地址。这样就可以通过基类指针，调用子类中重写的虚函数。

