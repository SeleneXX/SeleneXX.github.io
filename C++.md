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

### 构造函数

与python中的类的init类似。可以在类外定义

```c++
#include <iostream>
#include <string>

using namespace std;

class student{
private:
	int no;
    string name;
    float score[3];
public:
    student(); // 与类同名的构造函数
    // 可以重载构造函数
    void display(){
        cout << 1 << endl
    }
}

// 在类外定义构造函数，需要通过双冒号声明该函数的作用域，也就是目标类
student::student(){
    no = 0;
    name = "null";
    for(int i = 0; i < 3;i++){
        score[i] = 0;
    }
}

int main(){
    student s1;  // 定义对象的同时，构造函数自动调用
    return 0;
}
```

### 析构函数

与类同名，但是在函数前加上～。用于类对象生命期结束时回收对象。没有返回值和参数。只能有一个不能被重载。也可以在类外调用

```c++
class X{
    ~X(){
        cout << "Destroy\n" << endl;
    }
}
```

## 2. 函数重载

编译时的多态。函数名相同，但是传入的参数不同。

```c++
#include <iostream>

using namespace std;

void func(int a){
    count << "func1" << endl;
}

void func(int a, float b){
    count << "func2" << endl;
}

int main(){
    func(1);
    func(1, 1.1);
    return 0;
}
```

底层原理：

编译器实际将重载的同名函数进行名字碾碎成不同名，用于区分

```
void func(int i){}
编译后，名字变为Z4funci，4为函数名长度，后面func为函数名，i为参数类型int
void func(int i, int j){}
编译后，变为Z4funcii，因为有两个int类型的参数
```

## 3. C++代码运行过程

`.c .cpp`----预处理---->>`.i`----编译---->>`.s`----汇编---->>`.o`----链接---->>`.exe .out`

预处理：展开宏定义，删除注释等

编译：生成汇编语言

汇编：生成机器语言，.o是二进制文件

链接：生成可执行文件

## 4. new和malloc的区别

```
    malloc/free是库函数，需要头文件支持。

    new操作符申请内存分配时无须指定内存块的大小，编译器会根据类型信息自行计算。

    而malloc则需要显式地指出所需内存的尺寸。

    new操作符内存分配成功时，返回的是对象类型的指针，类型严格与对象匹配，无须进行类型转换，故new是符合类型安全性的操作符。

    而malloc内存分配成功则是返回void * ，需要通过强制类型转换将void*指针转换成我们需要的类型。

    new内存分配失败时，会抛出bac_alloc异常。

    malloc分配内存失败时返回NULL。

    C++允许重载new/delete操作符，

    而malloc不允许重载。

    new操作符从自由存储区（free store）上为对象动态分配内存空间，

    而malloc函数从堆上动态分配内存。

    自由存储区是C++基于new操作符的一个抽象概念，凡是通过new操作符进行内存申请，该内存即为自由存储区。而堆是操作系统中的术语，是操作系统所维护的一块特殊内存，用于程序的内存动态分配，C语言使用malloc从堆上分配内存，使用free释放已分配的对应内存。自由存储区不等于堆，如上所述，布局new就可以不位于堆中。
```



## 5. this指针

this指针是每个类内的成员函数默认的参数，保存对象的地址。

```c++
class CTest{
public:
    // 返回值为CTest对象指针的函数
    CTest* mumber_func(){
        // this保存当前对象的地址
        return this;
    }
}
```

