# CPP

##
* 函数存放位置：代码区
* 对象存放位置：堆（全局变量，可修改）、栈（局部变量，可修改）、全局数据区（不可修改）
* 对象访问方法：指针、引用、对象名

## Class
* 每一个类都应该有一个.cpp和.h文件；
* 所有的类的方法都会有一个this的指针；
* struct在某些情况下可以实现Class，但访问权限有所不同；
* Constructor：当对象被定义或者new时就会调用构造函数；
* Destructor：当对象被销毁或者delete时就会调用析构函数；

## Storage allocation
* 当程序进入到某个作用域时，会分配作用域内对象的内存空间，而只有程序运行到定义对象时才会调用构造器；

## Dynamic memory allocation
* new：new Class || new int || new int[]：return address；
* delete：delete var || delete[] var；
* 当程序运行时系统会自动分配一定内存给程序，而new出来的对象的内存空间是由程序分配的而不是系统分配的；
* new出来的对象的内存存在堆中；
* new出来的对象的内存空间大小（字节）和地址会被记录在entry table中；
* 当使用delete时会从entry table中寻找对象的内存大小和地址然后释放；
* 当new出一个数组时也需要使用delete[]去释放，否则只有数组第一个对象会调用析构函数，但空间会全部释放；

## Setting limits
* public：所有人都可以访问；
* private：只有这个类的成员函数可以访问；
* protected：只有自己以及子代可以访问；
* friend：授权别的类或方法可以访问自己的private；
* private限制只对自己有用，同个类的对象可以互相访问私有变量或函数；
* 访问权限是对于类来说而不是对象；
* 访问权限只有在编译时刻验证，运行时刻不进行验证；
* 类的访问权限默认为private，struct的访问权限默认为public；

## Initializer list
* 初始化列表早于构造函数执行；
* 类中所有成员变量都应该使用初始化列表进行初始化而不是在构造函数中；
* 若没有明确给出初始化列表也会进行初始化，只不过没有意义；
```bash
class A{
private:
        int i;
        int j;
public:
        A(xi,xj):i(xi),j(xj){}
};
```

## Reusing the implementation
### Composition
* Fully：包含其他对象，需要使用初始化列表对包含的其他对象进行构造，访问其他对象的变量和函数时仍然需要通过其他对象来访问和调用；
* By reference：不包含其他 对象但可以调用访问；
```cpp
class A{
public:
        A():b(){}       // 包含的其他对象需要使用初始化列表进行初始化而不是构造函数；
        ~A(){}
private:
        B b;    // Fully
};
```

## Reusing the interface
### Inheritance
* public的member functions和member data组成类的interface；
* 在一个类的基础上制造另一个类；
* 子类可以看作是父类的超集；
* 子类中需要在初始化列表中调用父类的构造；
* 父类的构造函数会早于子类调用；
* 父类的析构函数会晚于子类调用；
* 若子类中含有与父类相同参数相同名字的函数，则子类当中仅保留子类中定义的那个函数，而父类中若有相同名字不同参数的函数，子类中也不会保留；
* 若子类中含有与父类相同参数相同名字的函数，则这两个函数毫无关系；
```cpp
#include <iostream>
using namespace std;

class A{
public:
        A(int ii):i(ii){}
        ~A(){}
        void set(int i) {this->i = i;}
        void print(){ cout << i << endl;}
private:
        int i;
};

class B : public A{
public:
        B():A(10){}     // 子类中需要调用父类的构造函数；
        void f(){set(20);print();}      // 子类可以访问父类的函数；
private:
};

int main(){
        B b;
        b.set(10);
        b.print();
        b.f();
        return 0;
}
```

## Default arguments
* 不推荐使用，最好不要使用；
* 参数默认值的补全是由编译器完成的工作；
* 函数参数中可以给参数设置默认的值，则该函数拥有两种调用方法；
* 如果函数参数需要设置默认值，则需要在没有默认值参数的右边；
```c
// 声明
void Stash(int size,int initQuantity = 0);
// 定义
void Stash(int size,int initQuantity){}
Stash(10);      // 第一种调用，initQuantity默认为0；
Stash(10,10);   // 第二种调用；
```

## Inline Functions
* 将内联函数的代码内嵌到调用它的地方；
* 在编译器中进行操作，最后生成的可执行文件中不会有内联函数部分；
* 函数的声明和定义都应该说明为inline；
* 编译器一次只会编译一个.cpp文件，当inline函数分别在两个文件中进行定义和声明时，声明中并没有函数体，所以程序会尝试去调用inline函数而不是内嵌在已有代码中，而定义时由于为inline函数则编译器不会进行编译，当链接器将两个文件链接时就会出错；
* inline函数只需要声明，不需要定义；
* 如果编译器发现inline函数为递归或者函数太大，编译器可能会拒绝；
* 如果不为inline的小函数，编译器可能会自动将函数变为inline函数；
* 如果该函数频繁调用则可以为inline；
* 在.h文件中如果存在函数的定义，则这个函数就为inline函数；
* 可以将set()和get()函数为inline函数，和直接访问成员效率上没有区别；

```cpp
// .h
inline int plusOne(int x){return ++x;}
```

## Const
* const int bufsize = 1024;说明有一个const的bufsize，在哪里都不能被修改，而且必须初始化，赋给const的值必须明确不能为变量；
* extern const int bufsize;说明有一个bufsize在当前是为const，但在其他地方不一定为const；
* char的指针所指的字符串被当作常量看待被存放在代码段里，不可修改，而char类型的数组存放在堆栈中 可以修改；
* 在函数后面加上const表示this指针为const，这个函数不会将成员变量的值进行修改；

## Declaring references
* 引用本质上就是const的指针，不是一个实体；
* 引用不能重复引用；
* 不能对引用取地址；
* 引用必须要做初始值，初始值应该为变量，除非作为参数和成员变量；
* const int& z = x;     // 通过z不能改变x的值；
* 当引用作为参数传入函数时，在函数里可以直接对引用做修改；
* 当构造函数有一个参数为该类的对象的引用时，就可以使用该类的对象对一个新的对象初始化，进行成员对成员的拷贝，这称为拷贝构造；
```cpp
class A{
public:
        A(const A &a){}       // 拷贝构造
}
```

## Conversions
* 子类的对象可以交给父类对象；
* 子类的指针可以交给父类指针；
* 子类的引用可以交给父类引用；

## Polymorphism
* upcast和dynamic binding构成了多态；
* 如果函数前加上virtual并且子类中有函数名和参数相同的函数，则表示父类中的方法和子类的方法有联系；
* 如果通过指针或者引用调用virtual的函数则会调用该类型自己的函数而不是父类的；
* 只要一个类中存在一个virtual函数则该类的对象的内存空间最开始就会自动创建一个vtable（指针数组）并用一个隐藏的指针vptr指向它；
* vtable中的指针指向该类中的函数，，如果为virtual函数则指向自己的函数，如果不为virtual则指向父类的函数；
* 只要类中有一个virtual函数，则类的析构也必须是virtual；

## static
* 在函数内的static变量在第一次进入时初始化；
* 类里面的static的成员变量在每一个对象都是一致的；
* 类里面的static的成员变量必须在被定义；
* 类里面的statuc的成员变量不能在初始化列表做初始化，只能在定义的地方做初始化；

## Overloading Operators
* 会修改算子的不加const，不修改算子就加const；
```cpp
const Integer operator+(const Interger&n) const{}  // 重载Integer类的+运算符；
```

## Templates
* 类模板里面的每个函数都是函数模板；
```cpp
template < class T >    // 模板；
void swap(T& x,T& y){
        T temp = x;
        x = y;
        y = temp;
}

foo<int>();     // 将T设为int类型；
```

##

