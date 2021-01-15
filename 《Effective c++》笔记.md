## Item 01 ：视c++为一个语言联邦

***

认识c++主要的次语言:

* c
* Object-Oriented C++
* Template c++
* STL

## Item 02: 尽量以const，enum，inline替换#define

***

> 宁可以编译器替换预处理器，因为#define不被视为语言的一部分。

例如

```c++
//#define APECT_RATIO 1.653替换为
const double AspectRatio = 1.653;
```



作为一个语言常亮，AspectRatio肯定会被编译器看到，当然会进入记号表内。

当我们以常量替换#define，有两种特殊情况。

1. 定义常量指针。由于常量定义式通常被放在头文件内（以便被不同的源码含入），因此有必要将指针声明为const

   ```c++
   const std::string authorName("sxc");
   ```

2. 对于class专属常量，为了将常量的作用域限制于class内，让它成为class的一个成员；而为确保此常量至多只有一份实体，让它成为static成员。

   ```c++
   class GamePlayer{
   private:
       static const int Numturns = 5;
       int scores[Numturns];
       ...
   }
   ```

   如果编译器不支持上述，一个补偿做法"the enum hack"

   ```c++
   class GamePlayer{
   private:
       enum { NumTurns = 5};
       int score[Numturns];
       ...
   }
   ```



对于使用#define实现宏的情况。例如

```c++
#define CALL_WITH_MAX(a,b) f((a)<(b) ? (a):(b))
```

宏看起来像函数，但不会招致函数调用带来的开销。为了获得宏带来的效率和一般函数的所欲可预料行为或类型安全性。使用template inline函数

```c++
template<typename T>
inline void callwithMax(const T& a,const T& b)
{
    f(a>b ? a:b);
}
```

 

>  * 对于单纯变量，最好以const对象或enums替换#define
>
> * 对于形似函数的宏，改用inline函数替换#defines



## Item 03 尽可能使用const

***

> * 将某些东西声明为const可帮助编译器侦测出错误用法。const可被施加于任何作用域内的对象，函数实参，函数返回类型，成员函数本体。
> * 编译器强制实施bitwise constnesss，但你编写程序时应该使用“概念上的常量性''(conceptual constness)
> * 当const和non-const成员函数有着实质等价的实现时，令non-const版本调用const版本可避免代码重复。



## Item04 确定对象被使用前已先被初始化

***

最佳处理方法是：永远在使用对象之前先将他初始化

```c++
int x = 0;
const char* text = "A C-style string";
double d;
std::cin>>d;
```

对于内置类型以外的任何其他东西，初始化责任落在构造函数身上。

并使用member initialization（成员初始列）来编写构造函数

```c++
class PhoneNumber {...};
class ABEntry{
public:
    ABEntry(const std::string& name,const std::string& address,const std::list<PhoneNumber>& phones);
private:
    std::string theName;
    std::string theAddress;
    std::list<PhoneNumber> thePhones;
    int numTimesConsulted;
};
//成员初值列
ABEntry::ABEntry(const std::string& name,const std::string& address,const std::list<PhoneNumber>& phones)
    :theName(name),
     theAddress(address),
     thePhones(phones),
     numTimesConsulted(0)
{}
```

c++有着十分固定的“成员初始化次序”：base classes更早于其derive classes被初始化，而class的成员变量总是以其声明次序被初始化。

但是，c++对于“定义于不同编译单元内的non-local static对象”的初始化相对次序并无明确定义。但我们可以:将每个non-local static对象搬到自己的专属函数内（该对象在此函数内被声明为static），这些函数返回一个reference执行它所含的对象。然后用户调用这些函数，而不直接指涉这些对象。换句话说，non-local staic对象被local static对象替换了。

```c++
class FileSystem{
    public:
    ...
    sta::size_t numDisks() const;
    ...
};

//这里
FileSystem& tfs(){
    static FileSystem fs;
    return fs;
}

class Directory{
    public:
    Directory(params);
    ...
}
Directory::Diretory(params)
{
    ...
    std::size_t disks = tfs().numDisks();
}

//这里
Directory& tempDir()
{
    static Directory td;
    return td;
}
```

> * 为内置型对象进行手工初始化，因为c++不保证初始化他们
> * 构造函数最好使用成员初值列，而不要在构造函数本体内使用赋值操作。初值列列出的成员变量，其排列次序应该和它们在class中的声明次序相同
> * 未免除“跨编译单元之初始化次序”问题，以local static对象替换non-loal static对象



## Item05 了解c++默默编写并调用哪些函数

***

> 编译器可以暗自为class创建default构造函数，copy构造函数，copy assignment操作符以及析构函数



## Item06 若不想使用编译器自动生成的函数，就该明确拒绝

***

所有编译器产出的函数都是public，为阻止这些函数被创建出来，你可以自行声明他们，并令其为private.

**将成员函数声明为private而且故意不实现他们，被用在c++ iostream程序库中阻止copying行为**

```c++
class HomeForSale{
public:
...
private:
    ...
    HomeForSale(const HomeForSale&);//只有声明
    HomeForSale& operator=(const HomeForSale&);
}
```

将链接期错误转移至编译器的另一个方法

```c++
class Uncopyable{
    protected:
    Uncopyable(){}
    ~Uncopyable() {}
    private:
    Uncopyable(const Uncopyable&);
    Uncopyable& operator=(const Uncopyable&);
}

class HomeForSale:private Umcopyable{
    ...
};
```

> 为驳回编译器自动提供的机能，可将相应的成员函数声明为private并且不予实现。使用像Uncopyable这样的base class也是一种做法



## Item07  为多态基类声明virtual析构函数

***

给base class一个virtual析构函数，此后删除derived class对象会销毁整个对象，包括所有的derived class成分

```c++
class TimeKeeper{
    public:
    TimeKeeper();
    virtual ~TimeKeeper();
    ...
};

TimeKeeper *ptk = getTimeKeeper();
...
delete ptk;
```

如果class不含virtual函数，通常表示它并不意图被用作一个base class。



为你希望它成为抽象的那个class声明一个纯虚析构函数

```c++
class AWOV //abstract w/o virtuals
{
    public:
    virtual ~AWOV()=0;
}
AWOV::~AWOV(){ }
```

析构函数的运作方式是，最深层派生（most derived）的那个class其析构函数最先被调用，然后是其每一个base class的析构函数被调用。

> * polymorphic(带多态性质的)base classes应该声明一个virtual析构函数，如果class带有任何virtual函数，它就应该拥有一个virtual析构函数。
> * Classes的设计目的如果不是base classes使用，或不是为了具备多态性，就不该声明virtual析构函数



## Item08 别让异常逃离析构函数

***

如果你的析构函数必须执行一个动作，而该动作可能会在失败时抛出异常，这怎么办？

举个例子，假设你使用一个class负责数据库连接：

```c++
class DBConnection{
public:
    static DBConnection create();
    
    void close();
};
```

为确保客户不忘记在DBconnection对象身上调用close（），一个想法时创建一个用来管理DBConnection资源的class，并在析构函数中调用close

```c++
class DBConn{
public:
    ~DBConn(){
        db.close();
    }
    
private:
    DBConnetion db;
};
```

客户端代码

```c++
{
    DBConn dbc(DBConnection::create());
    ...
}
```

如果调用close导致异常，DBConn析构函数会传播该异常。这会造成问题。

一个较佳策略是重新设计DBConn接口，使其客户有机会对可能出现的问题做出反应。

```c++
class DBConn{
public:
    ...
    void close()
    {
        db.close();
        closed = true;
    }
    
    ~DBConn()
    {
        if(!closed){
            try{
                db.close();
            }catch(...){
                制作运转记录，记下对close的调用失败。
            }
        }
    }
    
private:
    DBConnection db;
    bool closed;
};
```

> * 析构函数绝对不要吐出异常，如果一个被析构函数调用的函数可能抛出异常，析构函数应该捕捉任何异常，然后吞下它们（不传播）或结束程序
> * 如果客户需要对某个操作函数运行期间抛出的异常做出反应，那么class应该提供一个普通函数（而非在析构函数中）执行该操作



## Item09：绝不在构造和析构过程中调用virtual函数

***

> 因为这类调用从不下降至derived class

 ##  Item10 ：令operate=返回一个reference to *this

***

```c++
class Widget{
    public:
    ...
        
    Widget& operator=(const Widget& rhs)
    {
        //....
        return* this;
    }
};
```

## Item11: 在operate= 中处理“自我赋值”

```c++
Widget& Widget::operate=(const Widget& rhs)
{
    Bitmap* pOrig = pb;
    pb = new Bitmp(*rhs.pb);
    delete pOrig;
    return *this;
}
```

copy and swap技术

```c++
class Widget{
    ,,,
    void swap(widget& rhs);
    ,,,
}

Widget Widget::operator=(const Widget& rhs)
{
    Widget temp(rhs);
    swap(temp);
    return *this;
}
```

> * 确保当对象自我赋值时 operate=  有良好行为。其中技术包括比较“来源对象”和“目标对象”的地址，精心周到的语句顺序，以及copy-and-swap
> * 确定任何函数如果操作一个以上的对象，而其中多个对象是同一个对象时，其行为仍然正确

## Item12:复制对象时勿忘其每一个成分

***

Customer表现顾客，其中手工写出（而非编译器创建copying函数。使得外界对它们的调用会被志记(logged)下来

```c++
void logCall(const std::string& funcName);//制造一个log entry

class Customer{
    public:
    ,,,
    Customer(const Customer& rhs);
    CUstomer& operate=(const Customer& rhs);
    ,,,
    private:
    std::string name;
}

class PriorityCustomer:public Customer{
public:
    ...
    PriPriorityCustomer(const PriorityCustomer& rhs);
    PriorityCustomer& operate=(const PriorityCustomer& rhs);
private:
    int priority;
};
```



为dervied class 编写copying函数的时候，也要复制其base class成分，那些成分往往是private，所以无法直接访问它们，应该让derived class的copying函数调用相应的base class函数

```c++
void logCall(const std::string& funcName);//制造一个log entry

class Customer{
    public:
    ,,,
    Customer(const Customer& rhs);
    CUstomer& operate=(const Customer& rhs);
    ,,,
    private:
    std::string name;
}

class PriorityCustomer:public Customer{
public:
    ...
    PriorityCustomer(const PriorityCustomer& rhs);
    PriorityCustomer& operate=(const PriorityCustomer& rhs);
private:
    int priority;
};

PriorityCustomer::PriorityCustomer(const PriorityCustomer& rhs)
:Customer(rhs),priority(rhs.private)
{
    logCall("PriorityCustomer copy constructor");
}

PriorityCustomer& PriorityCustomer::operate=(const PriorityCustomer& rhs)
{
    logCall("PriorityCustomer copy assignment operator");
    Customer::operator=(rhs);
    priority = rhs.priority;
    return *this;
}

```

> * Copying函数应该确保复制“对象内的所有成员变量”及“所有base class成分”
> * 不要尝试以某个copying函数实现另一个coping函数，应该将共同机能放进第三个函数中，并由两个coping函数共同调用。

## Item13 以对象管理资源

***

> * 为防止资源泄露，请使用RAII对象，他们在构造函数中获得资源并在析构函数中释放资源
> * 两个常被使用的RAII classes分别是tr1::shared_ptr和auto_ptr,复制动作会使它（被复制物）指向null

## Item14：在资源管理类中小心copying行为

***



## Item15：在资源管理类中提供对原始资源的访问

***



## Item16：成对使用new和delete时要采取相同形式

***

```c++
std::string* stringPtr1 = new std::string;
std::string* stringPtr2 = new std::string[100];
...
delete stringPtr1;
delete [ ] stringPtr2;
```



## Item17: 以独立语句将newed对象置入智能指针

***



## Item18： 让接口容易被正确使用，不易被误用

***

以函数替换对象,表现某个特定月份

```c++
class Month{
public:
    static Month Jan() { return Month(1);}
    static Month Feb() { return Month(2);}
    ...
    staic Month Dec() { return Month(12);}
    ...

private:
    explicit Month(int m);
    ...
}

Date d(Month::Mar(),Day(30),Year(1995)); 
```

## Item19: 设计class犹如设计ype

***



## Item20:  宁以pass-by-reference替换pass-by-value

***

 对于以下继承体系

```c++
class Person{
public:
    Person();
    virtual ~Person();
    ...
private:
    string name;
    string address;
};

class Student:public Person{
public:
    Student();
    ~Student();
    ...
private:
    string schoolName;
    string schoolAddress;
};
```

by value方式传递对象

```c++
bool validateStudent(Student s);
Student plato;
bool platoIsOk = validateStudent(plato);

```

对函数而言，参数的传递成本高，使用太多次构造和析构函数。

pass by refenerce-to-const可以回避所有那些构造和析构动作

```c++
bool validateStudent(const Student& s);
```

by refenerce方式传递参数也可以避免**slicing（对象切割）问题**

假设在一组classes上工作，用来实现一个图形窗口系统

```c++
class Window
{
public:
    ...
    string name() const;
    virtual void display() const;
};

class WindowWithScorllBars:public Window{
public:
    ...
    virtual void display() const;
}
```

使用 by refenerce-to-const的方式传递

```c++
void printNameAndDisplay(const Window& w)
{
    cout<<w.name();
    w.display();
}
```

传进来的窗口是什么类型，w就表现出那种类型

c++编译器底层，references往往以指针实现，因此pass-by-reference通常意味着真正传递的是指针。

> * 尽量以pass-by-reference-to-const替换pass-by-value。前者通常比较高效，并可避免切割问题（slicing problem）
> * 以上规则并不适用于内置类型，以及STL的迭代器和函数对象。对它们而言，pass-by-value更适当

## Item21 ：必须返回对象时，别妄想返回其reference

***

返回一个新对象的写法

```c++
inline const Rational operator*(const Rational& lhs,const Rational& rhs)
{
    return Rational(lhs.n*rhs.n,lhs.d*rhs.d);
}

```

## Item22: 将成员变量声明为private

***

从封装的角度观之，其实只有两种访问权限，private（提供封装）和其他（不提供封装）

## Item23: 宁以non-member，non-friend替换member函数

***

假设有个class表示网页浏览器

```c++
class WebBrowser{
public:
    ...
    void clearCache();
    void clearHistory();
    void removeCookies();
    ...
}
```

如果用户想一整个执行所有这些动作

1. webBrowser提供这样一个函数

```c++
class WebBrowser{
public:
    ...
    void clearEverthing();//调用clearcache，clear history，removecookies
    ...
}

```

2. 由一个non-member函数调用适当的member函数(更好)，因为它导致webBrowser class有较大的封装性

```c++
void clearBrowser(WebBrowser& wb)
{
    wb.clearCache();
    wb.clearHistory();
    wb.removeCookies();
}
```

另外一个自然的做法是让clearBrowser成为一个non-memeber函数并且位于WebBrowser所在的同一个namespace内

```c++
namespace WebBrowserStuff{
    class WebBrowser{
        ...
    }

    void clearBrowser(WebBrowser& wb);
    ...
}
```

namespace可以跨越多个源码文件而class不能



## Item24: 若所有参数皆需类型转换，请为此采用non-member函数

***



## Iten25：考虑写出一个不抛异常的swap函数

***



## Item26：尽可能延后变量定义式的出现时间

***



## Item27：尽量少做转型动作

***



## Item28：避免返回handles指向对象内部成分

***



## Item29：为”异常安全“而努力是值得的

***

假设有个class涌来表现夹带背景图案的GUI菜单，希望用于多线程环境，所以它有个互斥器作为并发控制之用

```c++
class PrettyMenu{
public:
    ...
    void changeBackground(istream& imgsrc);
    ...
private:
    Mutex mutex;
    Image* bgImage;
    int imagechanges;
};

void PrettyMenu::changeBackground(istream& imgsrc)
{
    Lock m1(&mutex);
    delete bgImage;
    ++imagechanges;
    bgImage = new Image(imgsrc);
}
```

lock class作为一种“确保互斥器被及时释放”的方法

## Item30： 了解inline

***

免除函数调用成本

## Item31：将文件间的编译依存关系降至最低

***



## Item32: 确定你的public继承是“is-a”关系

***

> public继承“意味着is-a，适用于base classes身上的每一件事情一定也适用于derived classes身上，因为每一个derived class对象也都是一个base class对象

## Item33：避免遮掩继承而来的名称

***

```c++
class Base{
private:
    int x;
public:
    virtual void mf1()=0;
    virtual vod mf1(int);
    virtual void mf2();
    void mf3();
    void mf3(double);
    ...
};

class Derived:public Base{
public:
    virtual void mf1();
    void mf3();
    void mf4();
    ...
};
```

“名称遮掩规则”，base class里的mf1和mf3被derived class内的mf1和mf3遮盖掉了

```c++
Derived d;
int x;
...
d.mf1();//ok dervied ::mf1
d.mf1(x);//Erroe!因为Derived::mf1遮掩了Base::mf1
d.mf2();//ok
d.mf3();//ok  derived::mf3
d.mf3(x);//Erroe!因为Derived::mf3遮掩了Base::mf3
```

可以使用using声明式达成目标

```c++
class Base{
private:
    int x;
public:
    virtual void mf1()=0;
    virtual vod mf1(int);
    virtual void mf2();
    void mf3();
    void mf3(double);
    ...
};

class Derived:public Base{
public:
    //using
    using Base::mf1;//让base class内名为mf1和mf3的所有东西
    using Base::mf3;//在dervied作用域内都可见（且public
    //
    virtual void mf1();
    void mf3();
    void mf4();
    ...
};
```

现在，继承机制将一如往昔地运作：

```c++
Derived d;
int x;
...
d.mf1();//ok dervied ::mf1
d.mf1(x);//ok base::mf1
d.mf2();//ok
d.mf3();//ok  derived::mf3
d.mf3(x);//ok.Base::mf3
```

假设derived以private继承base，而derived唯一想继承的mf1是那个无参数版本，using在这里派不上用场，因为using声明式会令继承而来的某给定名称之所有同名函数在derived class中都可见。**使用转交函数（forwarding function）**

```c++
class Base{
private:
    int x;
public:
    virtual void mf1()=0;
    virtual vod mf1(int);
    ...
};

class Derived:public Base{
public:
    virtual void mf1()
    { Base::mf1();}//转交函数暗自成为inline
    ...
};

Derived d;
int x;
...
d.mf1();//ok dervied ::mf1
d.mf1(x);//error!Base::mf1()被遮掩了
```

> * dervied classes内的名称会遮掩base classes内的名称，在public继承下从来没有人希望如此
> * 为了让被遮掩的名称再见天日，可使用using 声明式或转交函数

## Item34：区分接口继承和实现继承

***

考虑一个绘图程序中各种几何形状的class继承体系

```c++
class Shape{
public:
    virtual void draw() const = 0;
    virtual void error(const std::string& msg);
    int objectID() const;
    ...
};

class Rectangle: public Shape{...};
class Ellipse: public Shape{...};
```

纯虚函数draw使得shape成为一个抽象class，所以只能创建其derivedclasses的实体。

**首先考虑pure virtual函数draw函数**：

```c++
class Shape{
public:
    virtual void draw() const = 0;
    ...
};

```

pure virtual函数有两个突出特性：它们必须背任何“继承了它们”的具象class重新声明，而且它们在抽象class中通常没有定义。即

> 声明一个pure virtual函数的目的是为了让derived classes只继承函数接口

shape::draw给它的derived classes说“你必须提供一个draw函数，但我不干涉你怎么实现它”

**再考虑impure virtual函数**：

derived classes继承其函数接口，但impure virtual函数会提供一份实现代码，derivd classes可能覆写（override）它

> 声明（非纯）impure virtual函数的目的，是让dervied classes继承该函数的接口和缺省实现。

```c++
class Shape{
public:
    virtual void error(const std::string& msg);
    ...
};
```

 其接口表示，每个class都必须支持一个（“当遇上错误时可调用”的）error函数，但每个class可自由处理错误。如果某个class不想针对错误做出任何特殊行为，它可以退回到shape class提供的缺省错误处理行为。

shape:::error告诉derived classes：“你必须支持一个error函数，但如果你不想自己写一个，可以使用Shape class提供的缺省版本”

但是，允许impure virtual函数同时制定函数声明和函数缺省行为，也可能造成危险。

对于"提供缺省实现给derived classes，除非它们明白要求否则免谈"。即切断“virtual函数接口”和其”缺省实现“之间的连接，下面是一种做法

```c++
class Airport {...};
class AirPlane{
public:
    virtual void fly(const Airport& destination)=0;
    ...
protected:
    void defaultFly(const Airport& destination);
};

void AirPlane::defaultFly(const Airport& destination)
{
    //缺省行为，飞机飞到制定目的地
}

class ModelA: public AirPlane{
public:
    virtual void fly(const Airport& destination)
    { defaultFly(destination);}
    ...
};

class ModelB: public AirPlane{
public:
    virtual void fly(const Airport& destination)
    { defaultFly(destination);}
    ...
};

class ModelC: public AirPlane{
public:
    virtual void fly(const Airport& destination);
    ...
};
void ModelC::fly(const Airport& destination)
{
    //c型飞机得到执行目的地
}

```

上述fly和defaultFly，过度雷同的函数名称可能引起class命名空间污染问题。

下面是另一种方法

```c++
class Airport {...};
class AirPlane{
public:
    virtual void fly(const Airport& destination)=0;
    ...
};

void AirPlane::fly(const Airport& destination)
{
    //pure virtual实现
    //缺省行为，飞机飞到制定目的地
}

class ModelA: public AirPlane{
public:
    virtual void fly(const Airport& destination)
    { AirPlane::fly(destination);}
    ...
};

class ModelB: public AirPlane{
public:
    virtual void fly(const Airport& destination)
    { AirPlane::fly(destination);}
    ...
};

class ModelC: public AirPlane{
public:
    virtual void fly(const Airport& destination);
    ...
};
void ModelC::fly(const Airport& destination)
{
    //c型飞机得到执行目的地
}

```

这样fly的声明部分表现的是接口（那是derived classes必须使用的），其定义部分则表现出缺省行为。

**最后看shape里的non-virtual函数ObjectID：**

```c++
class Shape{
public:
    int objectID() const;
    ...
};
```

如果该函数是non-virtual。意味着它并不打算在dervied class中有不同的行为

> 声明non-virtual函数的目的是为了令derived classes继承函数的接口即一份强制性实现。

请记住：

> * 接口继承和实现继承不同。在public继承下，derived classes总是继承base classes的接口
> * pure virtual函数只具体指定接口继承
> * impure virtual函数具体指定接口继承及缺省实现继承
> * non-virtual函数具体指定接口继承以及强制性实现继承

## Item35 : 考虑virtual函数以外的其他选择

***

```c++

class GameCharacter{
public:
    virtual int healtValue() const;
}
```

healthValue并未定义为pure virtual，这表示将会有个计算健康指数的缺省算法。

考虑一些其他解法

### 由Non-virtual Interface手法实现Template Method模式

```c++
class GameCharacter{
public:
	int healthValue() const
	{
		...//做一些事前工作
		int retVal = doHealthValue();//做真正的工作
		...//做一些事后工作
		return retVal;
	}
	...
private:
	virtual int doHealthValue() const
	{
		...
	}
}
```

这一基本设计，即“令客户通过public non-virtual成员函数间接调用private virtual函数，称为non-virtual-interface（NVI）手法”，它是所谓Template Metho的设计模式的一个独特表现形式。把这个non-virtul函数（healthvalue）称为virtual函数的外覆器（wrapper）。

NVI手法的一个优点隐身在上述代码注释“事前工作”和“事后工作中”。这让代码保证在“virtual函数进行真正工作之前和之后""被调用，这意味着wrapper确保得以在一个virtual函数被调用之前设定好适当场景，并在调用结束后清理场景。

### 由Function Pointers 实现strategy模式

```c++
//forward declaration
class GameCharacter;
//缺省算法
int defaultHealthCalc(const GameCharacter& gc);
class GameCharacter{
public:
	typedef int (*HealthCalcFunc)(const GameCharacter&);
	explicit GameCharacter(HealthCalcFunc hcf = defaultHealthCalc):healthFunc(hcf){}
	int healthValue() const
	{
		return healthFunc(*this);
	}
	...
private:
	HealthCalcFunc healthFunc;
}
```

## Item36 绝不重新定义继承而来的non-virtual函数

***

## Item37 绝不重新定义继承而来的缺省参数值

***

理由：virtual函数是动态绑定，而缺省参数值是静态绑定。

### 复习：静态绑定与动态绑定

>  静态绑定，又称前期绑定，early binding；动态绑定，又称后期绑定，late binding。

对象的所谓**静态类型（static type），就是它在程序中被声明时所采用的类型。**

考虑以下class继承体系

```c++
class Shape{
public:
	enum ShapeColor{Red,Green,Blue};
	virtual void draw(ShapeColor color = red)const =0;
	...
}

class Rectangle:public Shape{
public:
	virtual void draw(ShapeColor color) const;
	...
}

class Circle:public Shape{
public:
	virtual void draw(ShapeColor color) const;
}
```

现考虑这些指针

```c++
Shape* ps;//静态类型为shape*
Shape* pc = new Circle;//静态类型为shape*
Shape* pr = new Rectangle;//静态类型为shape*
```

本例中，ps，pc，pr都被声明为pointer-to-shape类型，所以他们都以它为 静态类型。

【注】不论她们真正指向什么，它们的静态类型都是Shape*

对象的所谓动态类型（dynamic type）则是指**“目前对象所指的类型”**，也就是说，动态类型可以表现出一个对象将会有什么行为。pc动态类型是circle\*，pr是Rectangle\*，ps没有动态欸行，因为它尚未指向任何对象。

动态类型一如其名称，可以在程序执行过程中改变（通常是经由赋值动作）

```c++
ps = pc;//ps的动态类型如今是circle*
ps = pr;//.................rectangle*
```

virtual函数系动态绑定而来，意思是调用一个virtual函数时，究竟调用哪一份函数执行代码，取决于发出调用的那个对象的动态类型。

```c++
pc->draw(Shape::Red);//调用Circle::draw(Shape::red)
pr->draw(Shape::red);//调用Rectangle::draw(Shape::red)
```

使用NVI的一个替代手法

```c++
class Shape{
public:
	enum ShapeColor {Red,Green,Blue};
	void draw(ShapeColor color = Red) const
	{
		doDraw(color);
	}
	...
private:
	virtual void doDraw(ShapeColor color) const = 0;
};

class Rectangle:public Shape{
public:
	...
private:
	virtual void doDraw(ShapeColor color) const;//注意，不需要指定缺省参数值
}
```

由于non-virtual应该绝对不会被derived classes覆写，这个设计可以很清楚的使得draw函数的color缺省参数值总是Red。

## Item38 通过复合塑模出has-a或“根据某物实现出”

***

假设需要一个template，希望制造出一组classes用来表现由不重复对象组成的sets，可以使用标准程序库提供的set template

以下例子，Set对象可根据一个list对象实现出来

```c++
template<class T>
class Set{
public:
	bool member(const T& item) const;
	void insert(const T& item);
	void remove(const T& item);
	std::size_t size() const;
private:
	std::List<T> rep;
}

template<typename T>
bool Set<T>::member(const T& item) const
{
	return std::find(rep.begin(),rep.end(),item)!=rep.end();
}

template<typename T>
void Set<T>::insert(const T& item)
{
	if (!member(item))
	{
		rep.push_back(item);
	}
}

template<typename T>
void Set<T>::remove(const T& item)
{
	typename std::list<T>::iterator it = std.find(rep.begin(),rep.end(),item);
	if(it!=rep.end())
		rep.erase(it);
}

template<typename T>
std::size_t Set<T>::size() const
{
	return rep.size();
}
```

## Item 39 :明智而谨慎得使用private继承

***

private继承：

* 如果classes之间的继承关系是private，编译器不会自动将一个derived class对象转换为一个base class对象
* 由private base class继承而来的所有成员，在derived class中都会变成private属性

## Item40 多重继承

***

multiple inheritence：MI

# ch07 模板和泛型变成

***

## Item41 隐式接口和编译器多态

***

“运行期多态"和”编译器多态“，类似于”哪一个重载函数该被调用“（发生在编译器）和”哪一个virtual函数该被绑定）（发生在运行期）之间的差异。

 记住：

* classes 和template都支持接口和多态
* 对classses而言接口是显式的（explicit），以函数签名为中心，多态则通过virtual发生于运行期
* 对template参数而言，接口是隐式的（implicit），奠基于有效表达式。多态则通过template具象化和函数重载解析（function overloading resolution）发生于编译器

## Item42 了解typename的双重意义

***

## Item43 处理模板化基类内的名称

***

## Item44 将与参数无关的代码抽离templates

***

## Item45 运用成员函数模板接受所有兼容类型

```c++
template<typename T>
class Rational {
public:
	Rational(constT& numerator = 0,const T& denominator=1);
	const T numerator() const;
	const T denominator() const;
	...
};

template<typename T>
const Rational<T> operator*(const Rational<T>& lhs,const Rational<T>& rhs)
{
	...
}

```

## Item47 traits classes表现类型信息

## Item48 template元编程



# ch08 定制new和delete

***

## Item49 了解new-handler

***

当operator new抛出异常以反映一个未获满足的内存需求之前，它会先调用一个客户指定的错误处理函数，即new-handler。为了指定这个“用以处理内存不足”的函数，客户必须调用set_new_handler，那是声明与<new\>的一个标准程序库函数

```c++
namespace std{
	typedef void (*new_handler)();
	new_handler set_new_handler(new_handler p) throw();
}

```

可以这样使用set_new_handler

```c++
void outOfMem()
{
	std::cerr<<"Unable to satisfy request for memory\n";
	std::abort();
}

int main()
{
	std::set_new_handler(outOfMem);
	int* pBigDataArray = new int[10000000L];
    ...
}
```



































































































































