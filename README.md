## 引用与指针
 在底层，引用变量由指针按照常量的方式实现。与某对象绑定后不可更改。
* 引用与指针常量的关系
    1.  在内存中都占用4个字节。存放的都是被引用对象的地址，必须在定义的同时进行初始化。(__是否必须初始化，是否占用内存__)
    2.  指针常量本身允许寻址（以p为例），即&p返回指针常量本身的地址，指向对象用*p表示；引用变量本身（以r为例）不允许寻址，&r返回的是被引用对象的地址，而不是变量r的地址（r的地址由编译器掌握，程序员无法对它进行存取），被引用对象直接用r表示。（__是否可寻址__）
    3.  引用绑定之后不可变（__是否可变__）
    4.  引用只能一级，指针可以多级（__是否能为多级__）


## static

### 一、 static 修饰基本数据类型
参考： https://www.cnblogs.com/LUO77/p/5771237.html
1.  修饰局部变量
* 内存中的位置： 静态存储区
* 初始化： 局部的静态变量只能被初始化一次，且C中不可以用变量对其初始化，而C++可以用变量对其初始化。（c中对静态局部变量的初始化发生在代码执行前，编译阶段分配好内存后，就会对其进行初始化，所以C中无法使用变量对其进行初始化。而在C++中，初始化是在运行到相关代码时，主要原因是因为C++引入对象，要进行初始化需要执行响应构造函数）
* 作用域： 作用域仍为局部作用域，当定义它的函数或者语句块结束的时候，作用域随之结束。


注：  static用来修饰局部变量的时候，它就改变了局部变量的*存储位置*（从原来的栈中存放改为静态存储区）及其*生命周期*，但未改变其*作用域*。

2.  修饰全局变量
 * 内存中位置：静态存储区
 * 初始化：未经初始化的全局静态变量会被程序自动初始化为0
 * 作用域： 全局静态变量在它的声明文件之外是不可见的。准确的讲是从定义之处到文件结尾

 注： static修饰全局变量，未改变其存储位置及生命周期，而是改变了其作用域。使当前文件外的源文件无法访问。

### 二、 static 应用到函数中
修饰函数，使得函数只能在当前文件中使用。
### 三、 在类中的使用
##### 静态成员变量
static 声明的成员变量在类中只占一份内存，为所有对象共同拥有和维护。
1. 静态成员变量在类内进行声明，在类外进行定义和初始化。在类外进行初始化的时候，不能出现static 关键字和 private/protected/public访问规则。（__声明与初始化__）
2. 静态成员变量相当于类域中的全局变量，被类的所有对象共享，包括派生类。
3. 静态成员变量可以作为成员函数形参的默认值，普通成员变量不可以。不过也可以用const修饰static数据成员在类内初始化。
```C++
class base{ 
public : 
    static int _staticVar; 
    int _var; 
    void foo1(int i=_staticVar);//正确,_staticVar为静态数据成员 
    void foo2(int i=_var);//错误,_var为普通数据成员 
};
```
4. 静态数据成员的类型可以是所属类的类型，而普通数据成员不可以。普通数据成员只能声明成类的指针或引用。
```C++
class base{ 
public : 
    static base _object1;//正确，静态数据成员 
    base _object2;//错误 
    base *pObject;//正确，指针 
    base &mObject;//正确，引用 
};
```

##### static 静态成员函数：
普通成员函数中有隐藏的this指针。静态函数没有this指针。可以通过对象或类名调用。
1. 静态成员函数不能调用非静态成员变量或非静态成员函数。
2. 静态成员函数不能声明为虚函数、const、volate
3. 所以对象共享静态函数，不含this指针
4. 静态成员可以独立访问，无需创建任何对象实例就可以通过静态函数访问。
5. 当static 成员函数在类外定义时不需要static修饰。
6. 不可以同时使用const和static修饰成员函数
    C++编译器在实现const的成员函数时为了确保函数不能修改类的实例状态，会在函数中添加一个隐式的参数const this* .当一个成员函数为static时，是没有this指针的。

#### 在普通函数中
##### 静态变量与静态函数
1. static作用与局部变量时，改变了局部变量的生存周期。局部变量保存于栈上。栈上的内容只在函数的范围内存在，当函数运行结束后，这些内容也会被销毁。自动全局变量和static变量（包括static全局和局部变量）保存于静态区，静态区的内容在程序整个生命周期都存在，由编译器分配。
2. static 作于用全局变量和函数，使得全局变量和函数只在本文件有效。

## const
参考：https://blog.csdn.net/Eric_Jo/article/details/4138548?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param
### 一、 const 修饰基本数据类型

### 二、 const 应用到函数中
1. const 修饰函数参数
2. const 修饰函数返回值
通常用于操作符重载。（防止计算结果被赋值。
### 三、 类相关const
1. const 修饰成员变量
const修饰类的变量，表示成员常量。不能被修改，**同时只能在初始化列表中赋值。（如果不在初始化列表中赋值，则会默认初始化再赋值，违背const原则）**
2. 修饰成员函数
    * const 修饰类的成员函数，则该成员函数不能修改类中的任何成员变量。一般写在函数的最后来修饰。
    ```c++
    class A
    {
        ...
        void function() const;//常成员函数，不能改变对象的成员变量。
                            //也不能调用非常成员函数；
    }
    ```
    *  对于const类对象/指针/引用，只能调用const成员函数，
3. const 修饰类对象/对象指针/对象引用
    * const修饰类对象表示该对象为常量对象。不能修改其成员对象。对于对象指针和对象引用也一样
    * const修饰的对象。不能调用该对象的非const成员函数。因为非const成员函数可能修改成员变量。
    * 当调用常对象的默认初始化列表时。对象必须存在默认构造函数。
    
## new与delete
参考：https://www.cnblogs.com/qg-whz/p/5140930.html
* **属性**
 malloc/free 是库函数，需要头文件的支持；new/delete是关键字，需要编译器支持。
* **申请内存的位置**
 new操作符从自由存储区（free store）上为对象分配内存，而malloc函数从堆上动态分配内存。
* **是否需要指定内存大小** 
使用new操作符申请内存时无需指定内存大小，编译器会根据类型信息自行计算，而malloc需要显示指定出所需内存大小。
```cpp
class A{...}
A* ptr = new A;
A* ptr == (A *)malloc(sizeof(A))；//需要显示指定内存所需大小
```
* **返回类型安全性**
new操作内存分配成功，返回的是对象类型的指针，类型与对象匹配，无须进行类型转换，故new是符合类型安全的操作符。而malloc内存分配返回的是void *，需要通过亲啊复制类型转换将void* 指针转换成我们需要的类型/
* **内存分配失败时的返回值**
new 内存分配失败时，会抛出bac_alloc异常，不会返回NULL；malloc分配内存失败时会返回NULL。
* **是否调用构造函数**
参考：https://blog.csdn.net/fengbingchun/article/details/78991749
使用new操作符分配对象有三个过程：
    * 调用operate new函数（对于数组则是operate new[]）的标准库函数。该函数分配一个原始的、足够大的、未命名的内存空间以便存储特定内存对象（或者对象数组)。第二步编译器运行响应构造函数以构造这些对象，并为其传入初值。第三步，对象被分配了空间并构造完成，返回一个指向该对象的指针。


当调用delete表达式删除一个动态分配的内存时。
```cpp
delete sp;
delete[] arr;
```
实际上执行两步：
    * 第一步：对sp所指的对象或者arr所指的数组中的元素执行对应的析构函数。第二步：编译其调用名为 operator delete(或者operate delete[])的标准库函数释放空间。


## 虚函数virtual
 声明基类的指针，利用该指针指向一个子类对象，调用相应的函数。
 * ** 纯虚函数 **
    * 在基类中声明的虚函数，在基类中没有定义。但要求派生类实现自己定义的实现方法。
    * 包含纯虚函数的类称为抽象类。
    * 如果父类或者祖先类中函数func()为虚函数，则其子类及其后代类中函数func（）是否加virtual都将是虚函数。为提高程序可读性，建议后代虚函数中都加上vitual 关键字。


* 函数重写使得虚函数不能静态编译
* 虚函数的分配在运行时
* 对每一个继承或者定义了虚函数的类，编译器都会生产虚表
* 虚指针指向虚表
* 将基类的析构函数声明为虚函数

* 普通函数（非类函数）不能是虚函数
* 静态函数（static）不能是虚函数（静态函数）
    * 静态与非静态成员函数之间有一个主要的区别。静态成员函数没有this指针。
    
    * 虚函数依靠vptr和vtable来实现。vptr是一个指针，在楼的构造函数中创建生成（这也是为什么构造函数不能声明为虚函数的原因）。并且能用this指针访问它，因为它是类的一个成员，并且vptr指向保存虚函数地址的vtable。
    * 对于静态函数成员，因为它没有this指针，所以无法访问vptr，这就是为何static函数不能为virtual。
    **虚函数的调用关系： this->vptr->vtable->virtual function**


* **虚析构函数**
    * 虚析构函数的作用： 基类的指针指向派生类的对象，并用基类指针删除派生类的对象。
    * 如果基类下析构函数不加virtual关键字
    若基类的析构函数不声明成虚析构函数时，当定义一个基类的指针并指向子类时，delete掉指针时。只调用基类的析构函数，而不调用子类的析构函数。造成内存泄漏
    * 如果基类的析构函数加virtual关键字
    此时调用子类的析构函数，再调用父类的析构函数


## this指针
* `this` 指针指示的是一个隐含于每一个非静态类成员函数中的静态指针。它指向调用该成员函数的哪个对象。
* 当对一个对象调用成员函数时，编译程序先将对象地址赋给`this`指针，然后调用成员函数。每次成员函数存取数据成员时，都隐式使用`this`指针
* `this`指针被隐式声明为：`ClassName *const this`，这意味不能给`this`赋值；在`ClassName`类的`const`成员函数中，`this`指针的类型为：`const Class * const`,这说明`this`指针所指向的对象不可修改（即不能对这种对象的数据成员进行赋值操作）；
* `this`并不是一个常规变量，而是一个右值（临时变量），所以不能去`this`地址（不能&this）

## inline 内联函数
* 相当于把内联函数的内容写在调用函数处
* 相当于不进入函数体内，直接执行函数体
* 相当于宏，比宏多了类型检查，真正具有函数特性
* 编译器一般不内联包含循环、递归、switch等复杂操作的内联函数
* 在类中定义了的函数，除了虚函数的其他函数都会自动隐式地当成内联函数。在内为定义可以显示内联
```c++
class A{
    int dog();
}
inline int A::dog(){return 0;}
```
**编译器对inline的处理步骤
1. 将inline函数体复制到调用处
2. 为所用inline函数的局部变量分配存储空间；
3. 将inline函数的输入参数和返回值映射到调用方法的局部变量空间
4. 如果inline有多个返回点，将其变为inline函数代码块状末尾的分支（使用GOTO)

**优缺点**
*优点*
1. 内联函数同宏函数一样将在被调用处展开，省去了参数压栈、栈帧开辟与回收，结果返回等，从而提高程序运行速度
2. 内联函数相对于宏来说，在代码展开时，会进行安全检查于自动类型转换（同普通函数一样），而宏不会
3. 在类中声明同时定义的成员函数，自动转换为内联函数，因此内联函数可以访问成员变量，宏则不可以
* 内联函数可以调试，宏则不行
*缺点*
1. 代码膨胀。内联函数是以代码复制为代价，消除函数调用带来的开销。如果执行函数体内的时间相对于函数调用开销大，效率会很少。另一方面，每一处内联函数的调用都要赋值代码，程序总代码量增加，消耗更多的内存空间
2. 内联函数无法随着函数库的升级而升级。
3. 是否内联，程序员不可控。 内联函数只是建议，是否内联，取决于编译器

** 虚函数（virtual）可以内联（inline）吗
* 虚函数可以是内联函数，内联可以修饰虚函数。但当虚函数表现多态性的时候没有内联
* 内联是在编译时建议编译器内联，而虚函数的多态性是在运行期，编译器无法知道运行期调用了哪一个代码。因此虚函数表现为多态性时（运行期）不能内联
* `inline virtual` 唯一可以内联的时候是：编译器知道所调用对象是哪一个类（如`Base::who()`）。这只有在**编译器具有实际对象而不是对象指针或引用时才会发生**
```cpp
#include <iostream>  
using namespace std;
class Base
{
public:
	inline virtual void who()
	{
		cout << "I am Base\n";
	}
	virtual ~Base() {}
};
class Derived : public Base
{
public:
	inline void who()  // 不写inline时隐式内联
	{
		cout << "I am Derived\n";
	}
};

int main()
{
	// 此处的虚函数 who()，是通过类（Base）的具体对象（b）来调用的，编译期间就能确定了，所以它可以是内联的，但最终是否内联取决于编译器。 
	Base b;
	b.who();

	// 此处的虚函数是通过指针调用的，呈现多态性，需要在运行时期间才能确定，所以不能为内联。  
	Base *ptr = new Derived();
	ptr->who();

	// 因为Base有虚析构函数（virtual ~Base() {}），所以 delete 时，会先调用派生类（Derived）析构函数，再调用基类（Base）析构函数，防止内存泄漏。
	delete ptr;
	ptr = nullptr;

	system("pause");
	return 0;
} ```

## const与 define
1. define 定义的变量在内存中有若干分备份。 const 定定义的变量只有一份