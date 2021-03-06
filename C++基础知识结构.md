# C++
文章：https://github.com/linw7/Skill-Tree/blob/master/%E7%BC%96%E7%A8%8B%E8%AF%AD%E8%A8%80C%2B%2B.md

---

## 一个基本C++类
```cpp

class B
{
public:
     B() :data(0)    //默认构造函数  
    {
        cout << "Default constructor is called." << endl;
    }
    B(int i) :data(i) //带参数的构造函数  
    {
        cout << "Constructor is called." << data << endl;
    }
    B(const B &b)   // 复制（拷贝）构造函数  
    {
        data = b.data; 
        cout << "Copy Constructor is called." << data << endl;
    }
    B(B&& b)      // 移动构造函数，严格意义上移动构造函数的作用是，this去接管b的资源，同时b的资源被销毁
    {
        this->data = b.data;
        b.data=nullptr;
        cout << "Move Constructor is called." <<data<< endl;
    }
    B& operator = (const B &b) //赋值运算符的重载  
    {
        this->data = b.data;
        cout << "The operator \"= \" is called." << data << endl;
        return *this;
    }
    ~B() //析构函数  
    {
        cout << "Destructor is called. " << data <<" "<<&data<< endl;
    }
private:
    int data;
};
B fun(B c)
{
    return c;
}

文章：https://blog.csdn.net/haolexiao/article/details/53465534

```

---

## c++内存
**C/C++程序编译时内存分为5大存储区**
（1）栈区（stack）：由编译器自动分配释放，存放函数的参数值，局部变量值等，其操作方法类似数据结构中的栈。
（2）堆区（heap）：一般由程序员分配释放，与数据结构中的堆毫无关系，分配方式类似于链表。
（3）全局/静态区（static）：全局变量和静态变量的存储是放在一起的，在程序编译时分配。
（4）文字常量区：存放常量字符串。
（5）程序代码区：存放函数体（类的成员函数、全局函数）的二进制代码。

```
int a=0; //全局初始化区
char *p1; //全局未初始化区
void main()
{
	int b; //栈
	char s[]="bb"; //栈
	char *p2; //栈
	char *p3="123"; //其中，“123\0”常量区，p3在栈区
	static int c=0; //全局区
	p1=(char*)malloc(10); //10个字节区域在堆区
	strcpy(p1,"123"); //"123\0"在常量区，编译器 可能 会优化为和p3的指向同一块区域
｝
```
**C/C++内存分配有三种方式：**
（1）从静态存储区域分配。内存在程序编译的时候就已经分配好，这块内存在程序的整个运行期间都存在。例如全局变量，static变量。
（2）在栈上创建。在执行函数时，函数内局部变量的存储单元都可以在栈上创建，函数执行结束时这些存储单元自动被释放。
栈内存分配运算内置于处理器的指令集中，效率很高，但是分配的内存容量有限。
（3）从堆上分配，亦称动态内存分配。程序在运行的时候用malloc或new申请任意多少的内存，程序员自己负责在何时用free或delete释放内存。
动态内存的生存期由程序员决定，使用非常灵活，但如果在堆上分配了空间，就有责任回收它，否则运行的程序会出现内存泄漏。
另外频繁地分配和释放不同大小的堆空间将会产生堆内碎块。

## c++对象模型
https://www.cnblogs.com/skynet/p/3343726.html
## c++语言关键字
### #define
这是来源于c的一个概念，即宏定义，宏定义的本质是替换，在程序中表现为替换文本，它可以作为常量的表示方式，也可以有宏定义函数的表现方式
```cpp
#define CHECK(arg){                 \
                                    \
    cout<<"CHECK:"<<arg<<endl;      \
}
 
static int _T;
#define CHECKOPENMP(arg)    \
{                           \
    _T = 10;                \
}

```
在其反斜杠\后面不能加其他东西，注释也不能。
以上是关于宏的一些应用。既然宏是一种文本替换，那么它的替换过程发生在什么阶段呢？是的，它发生在预处理阶段--该阶段会进行文本替换，将所有的#符开头的一些定义和引用在程序文本合适的位置中进行文本替换，所以宏因此也有一个显著特点，也即**不会进行类型检查**。
### const
const作为一种修饰符的存在，它限定了变量为不可更改的，所以用const修饰的变量必须在定义时初始化，并且作为本质上还是修饰的作用，而他的针对对象作为一种类型数据在使用时是会进行类型检查的，而不像宏只是一个替换操作。同时，宏是会存在多个拷贝内存，而const修饰下只会指向一个真实的地址空间，即只会存在一个内存拷贝。而分析它的存储区能知道，有两种情况，一种存储在只读数据段（全局变量或者external修饰值），一种放在符号表区，不分配内存（像const a=10;）。
```
作用:
修饰变量，说明该变量不可以被改变；
修饰指针，分为指向常量的指针和指针常量；
常量引用，经常用于形参类型，即避免了拷贝，又避免了函数对值的修改；
修饰成员函数，说明该成员函数内不能修改成员变量。
```
### static
static作用下他的默认初始化为0。
用法：
static的作用主要有以下3个：
1、扩展生存期；
2、限制作用域；
3、唯一性；
```
扩展生存期:
    这一点主要是针对普通局部变量和static局部变量来说的。声明为static的局部变量的生存期不再是当前作用域，而是整个程序的生存期。
    **static变量，管是局部还是全局，都存放在静态存储区**。
ps:局部变量的默认类型都是auto，从栈中分配内存。
```
```
限制作用域:
    static全局变量和函数，其作用域为当前cpp文件，其它的cpp文件不能访问该变量和函数。如果有两个cpp文件声明了同名的全局静态变量，那么他们实际上是独立的两个变量.也可以称为“隐藏特性”.
ps：static函数的好处是不同的人编写不同的函数时，不用担心自己定义的函数，是否会与其它文件中的函数同名。
```
```
数据唯一性:
    这是C++对static关键字的重用。主要指静态数据成员／成员函数。表示属于一个类而不是属于此类的任何特定对象的变量和函数. 这是与普通成员函数的最大区别, 也是其应用所在, 比如在对某一个类的对象进行计数时, 计数生成多少个类的实例, 就可以用到静态数据成员. 在这里面, static既不是限定作用域的, 也不是扩展生存期的作用, 而是指示变量/函数在此类中的唯一性. 这也是”属于一个类而不是属于此类的任何特定对象的变量和函数”的含义. 因为它是对整个类来说是唯一的, 因此不可能属于某一个实例对象的. (针对静态数据成员而言, 成员函数不管是否是static, 在内存中只有一个副本, 普通成员函数调用时, 需要传入this指针, static成员函数调用时, 没有this指针. )
```
### extern
1、基本解释：
extern 可以置于变量或者函数前，以标示变量或者函数的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块中寻找其定义。此外，extern 也可以用来链接指定。
extern 有两个作用：
(1) 当它与"C"一起连用时，如：extern “C” void func(int a)；则告诉编译器在编译 func 函数名时按着 C 的规则去翻译相应的函数名而不是 C++ 的。关于这点，或许可以在《深度探索C++对象模型》一书中寻找答案；另外，在 Linux 下有一个 backtrace 函数可以打印堆栈信息，可以查看 C++ 翻译的函数名(muduo 库中有使用这个函数)。
(2) 当 extern 不与 “C” 在一起修饰变量或者函数时，如在头文件中：extern int g_Int; 它的作用就是声明函数或全局变量的作用范围的关键字，其声明的函数和变量可以在本模块或者其他模块中使用，记住它是一个声明不是定义！
2、当 extern 修饰变量时
正确使用方法是：在 .c 文件中定义变量，在相应的 .h 文件中进行声明。
我们通过是否会为变量来分配内存空间来判定是声明还是定义(严格来说，是单纯的分配内存，并不包括初始化部分)。那么 int i; 这句话是声明还是定义那？它既是声明，也是定义。如果我们在 test.h 文件中使用这句话，一旦在其他文件中定义 i(e.g.1)，或者该文件被重复包含(e.g.2)，那么就会产生重定义的错误。
### explicit
该关键字，一般伴随着C++类而存在。常见的作用是：**阻止不应该允许的经过转换构造函数进行的隐式转换的发生**。
```cpp
class ExplicitNo{
public:
    ExplicitNo(int val);
    ~ExplicitNo();
};

class Explicit{
public:
    explicit Explicit(int val); 
    ~Explicit();
};
```
### inline
**inline关键字必须和函数体定义放在一起才可以实现内联，仅仅将inline放在函数声明之前不起任何作用。inline是一个用于实现的关键字而不是一个用于声明的关键字。对于类方法，定义在类体内部的方法自动成为内联方法。**
内联函数的**基本思想**在于将每个函数调用以它的代码体来替换，很可能会增加整个目标代码的体积过分地使用内联所产生的程序会因为有太大的体积而导致可用空间不够。即使可以使用虚拟内存，内联造成的代码膨胀也可能会导致不合理的页面调度行为（系统颠簸），这将使你的程序运行慢得象在爬，过多的内联还会降低指令高速缓存的命中率，从而使取指令的速度降低，因为从主存取指令当然比从缓存要慢。另一方面，如果内联函数体非常短，编译器为这个函数体生成的代码就会真的比为函数调用生成的代码要小许多。如果是这种情况，内联这个函数将会确实带来更小的目标代码和更高的缓存命中率！
inline指令就象register，它只是对编译器的一种提示，而不是命令。也就是说，只要编译器愿意，它就可以随意地忽略掉你的指令，事实上编译器常常会这么做。例如，大多数编译器拒绝内联"复杂"的函数（例如，包含循环和递归的函数）；还有，即使是最简单的虚函数调用，编译器的内联处理程序对它也爱莫能助。（这一点也不奇怪。virtual的意思是"等到运行时再决定调用哪个函数"，inline的意思是"在编译期间将调用之处用被调函数来代替"，如果编译器甚至还不知道哪个函数将被调用，当然就不能责怪它拒绝生成内联调用了）。
详细来源文章：https://www.cnblogs.com/alex-tech/archive/2011/03/24/1994273.html
### sizeof
sizeof不是函数，而是一个**单目运算符**，sizeof是一个操作符（operator）。其作用是返回一个对象或类型所占的内存字节数。其返回值类型为size_t。（size_t在头文件stddef.h中定义，它依赖于编译系统的值，一般定义为 typedef unsigned int size_t;）。
```cpp
//ANSI　C正式规定字符类型为1字节。　   
    sizeof(char)          = 1;
    sizeof(unsigned char) = 1;
    sizeof(signed char)   = 1;
//以下类型ANSI没有规定，依赖于OS和编译器，但大小一般差不多　　　   
    sizeof(int)            = 4;
    sizeof(unsigned int)   = 4;
    sizeof(short int)      = 2;
    sizeof(unsigned short) = 2;
    sizeof(long int)       = 4;
    sizeof(unsigned long)  = 4;
    sizeof(float)          = 4;
    sizeof(double)         = 8;
```
### new和delete
new和delete都属于运算操作符，不是函数。new的底层实现是mallol，它们是动态创建内存，所以都是在堆上实现。在这些简单过程中，还有一个小细节，当用new创建数据类型时，开辟的空间大小在原数组大小上还会增加4字节来来管理整个数组存储多少个对象。这样delete[]时，便会字到要执行多少个析构函数了。

malloc/free和new/delete的区别和联系
> 
　　*  malloc/free只是动态分配内存空间/释放空间。而new/delete除了分配空间还会调用构造函数和析构函数进行初始化与清理（清理成员）。

　　*  它们都是动态管理内存的入口。
　　*  malloc/free是C/C++标准库的函数，new/delete是C++操作符。
　　*  malloc/free需要手动计算类型大小且返回值w为void*，new/delete可自动计算类型的大小，返回对应类型的指针。

　　*  malloc/free管理内存失败会返回0，new/delete等的方式管理内存失败会抛出异常。

### volatile
volatile 关键字是一种类型修饰符，用它声明的类型变量表示可以被某些编译器未知的因素（操作系统、硬件、其它线程等）更改。所以使用 volatile 告诉编译器不应对这样的对象进行优化。
volatile 关键字声明的变量，每次访问时都必须从内存中取出值（没有被 volatile 修饰的变量，可能由于编译器的优化，从 CPU 寄存器中取值）
const 可以是 volatile （如只读的状态寄存器）
指针可以是 volatile
### struct与class
![](../picg/struct与class的区别.png)
### virtual
virtual作为虚函数的实现关键字用于动态绑定，所以要理解virtual也即要理解虚函数相关知识。
虚函数作为实现多态性的一种方式，通过虚指针和虚表这两个结构实现。具体实现过程如下：
```
这是一种常见方法，每个基类有一个虚函数表，每个类对象有一个虚指针，虚指针指向虚函数表地址。当用基类对象变量指向子类对象变量时，其内存中的数据存储内容等没变，但是按照基类对象的访问方式进行访问，这时候通过的虚指针访问虚函数表的实现时会访问子类的虚函数表。
```

虚函数相关问题可以查阅 https://blog.csdn.net/shuzfan/article/details/77165474

### override
1.在函数比较多的情况下可以提示读者某个函数重写了基类虚函数（表示这个虚函数是从基类继承，不是派生类自己定义的）；
2.强制编译器检查某个函数是否重写基类虚函数，如果没有则报错。

用法：在类的成员函数参数列表后面添加该关键字既可。
### 四种类型转换
- static_cast：
  可以实现C++中内置基本数据类型之间的相互转换。如果涉及到类的话,static_cast只能在有相互联系的类型中进行相互转换,不一定包含虚函数。
- const_cast：
  const_cast操作不能在不同的种类之间转换，它仅仅把一个它作用的表达式转换成常量,它可以使一个本来不是const类型的数据转换成const类型,或者把const属性去掉。
- reinterpret_cast：
  有着和C风格的强制转换同样的能力,它可以转换任何内置的数据类型为其它任何的数据类型,也可以转换任何指针类型为其它的类型,它甚至可以转换内置类型为指针,无须考虑类型安全或者常量的情况,**不到万不得已绝不使用**。
- dynamic_cast:
  (1) 其它三种都是编译时完成的,dynamic_cast是运行时处理的,运行要进程类型检查.
  (2)不能用于内置的基本数据类型的强制转换.
  (3)dynamic_cast转换如果成功的话返回的是指向类的指针或引用，转换失败的话则返回NULL.
  (4)使用dynamic_cast进行转换的,基类中一定要有虚函数,否则编译不通过.B中需要检测有虚函数的原因:类中存在虚函数,就说明它有想要让基类指针或引用指派生类对象的情况，此时转换才有意义.这是由于运行时类型检查需要运行时类型信息,而这个信息存储在类的虚函数表中(关于虚函数表的概念)中.只有定义了虚函数的类才有虚函数表.
  (5)在类的转换时,在类层次之间进行上行转换时,dynamic_cast和static_cast的效果是一样的，在进行下行转换时，dynamic_cast具有类型检查的功能,比static_cast更安全,向上转换即为指向子类对象的向下转换,即将父类指针转换为子类指针,向下转换的成功与否还与将要转换的类型有关,即要转换的指针指向的对象的实际类型与转换以后的对象类型一定要相同,否则转换失败.

## c++11相关知识
文章：https://blog.csdn.net/iEearth/article/details/78937837。
1、类型声明decltype
C++11引入的decltype是对auto的补充，auto告诉编译器根据表达式推断变量的类型并由这个表达式对变量进行初始化，而decltype与auto的区别是初始化使用的是另外一个表达式，初始化可以在定义之后进行，decltype根据表达式推断类型时这个表达式并没有任何的副作用，如使用函数的返回值类型而不会真正去调用这个函数。
2、自动类型auto
C++11引入了自动类型声明符auto，使用auto类型时必须进行初始化，auto可以表示任意类型，具体什么类型由编译器根据初始化表达式进行推断，见上面的例子。使用auto定义多个变量时，只能涉及一个基本类型，保持彼此类型的一致性，否则是错误的，指针和引用不是基本类型，见上面的例子。
3、Lambda表达式
说到匿名函数可能就要涉及到```std::function & std::bind```,它定义了一个匿名函数，可以捕获一定范围的变量在函数内部使用，一般有如下语法形式：
```cpp
auto func = [capture] (params) opt -> ret { func_body; };
```
其中func是可以当作lambda表达式的名字，作为一个函数使用，capture是捕获列表，params是参数表，opt是函数选项(mutable之类)， ret是返回值类型，func_body是函数体。
- []不捕获任何变量
- [&]引用捕获，捕获外部作用域所有变量，在函数体内当作引用使用
- [=]值捕获，捕获外部作用域所有变量，在函数内内有个副本使用
- [=, &a]值捕获外部作用域所有变量，按引用捕获a变量
- [a]只值捕获a变量，不捕获其它变量
- [this]捕获当前类中的this指针

std::function:
std::function作为可调用对象的封装器，用于表示函数这个抽象概念，std::function的实例可以存储、复制和调用任何可调用对象，存储的可调用对象称为std::function的目标，若std::function不含目标，则称它为空，调用空的std::function的目标会抛出std::bad_function_call异常。
可调用对象:
- 是一个函数指针
- 是一个具有operator()成员函数的类对象(传说中的仿函数)，lambda表达式
- 是一个可被转换为函数指针的类对象
- 是一个类成员(函数)指针
- bind表达式或其它函数对象
std::bind:
使用std::bind可以将可调用对象和参数一起绑定，绑定后的结果使用std::function进行保存，并延迟调用到任何我们需要的时候。
std::bind通常有两大作用：
- 将可调用对象与参数一起绑定为另一个std::function供调用
- 将n元可调用对象转成m(m < n)元可调用对象，绑定一部分参数，这里需要使用std::placeholders。
4、智能指针
shared_ptr：
shared_ptr使用了引用计数，每一个shared_ptr的拷贝都指向相同的内存，每次拷贝都会触发引用计数+1，每次生命周期结束析构的时候引用计数-1，在最后一个shared_ptr析构的时候，内存才会释放。
关于shared_ptr有几点需要注意：
- 不要用一个裸指针初始化多个shared_ptr，会出现double_free导致程序崩溃
- 通过shared_from_this()返回this指针，不要把this指针作为shared_ptr返回出来，因为this指针本质就是裸指针，通过this返回可能 会导致重复析构，不能把this指针交给智能指针管理。
- 尽量使用make_shared，少用new。
- 不要delete get()返回来的裸指针。
- 不是new出来的空间要自定义删除器。
- 要避免循环引用，循环引用导致内存永远不会被释放，造成内存泄漏。

weak_ptr：
weak_ptr是用来监视shared_ptr的生命周期，它不管理shared_ptr内部的指针，它的拷贝的析构都不会影响引用计数，纯粹是作为一个旁观者监视shared_ptr中管理的资源是否存在，可以用来返回this指针和解决循环引用问题。
作用1：返回this指针。
作用2：解决循环引用问题。
std::unique_ptr：
std::unique_ptr是一个独占型的智能指针，它不允许其它智能指针共享其内部指针，也不允许unique_ptr的拷贝和赋值。使用方法和shared_ptr类似，区别是不可以拷贝：
5、右值引用
表达式的值有三种类别：左值、右值和临终值。 其中左值是指在表达式的外部保留的对象，可以将左值视为有名称的对象，所有的变量都是左值。 右值是一个不在使用它的表达式的外部保留的临时值，比如函数的返回值、字面常量。 临终值是生命周期已经结束，但内存仍未回收的值，比如函数的返回值可以声明为int&&。 若要更好地了解左值和右值之间的区别，看下面的示例：
```cpp
int a = 3;              // a 是变量，所以它是一个左值
                        // 3 是字面常量，所以它是一个右值
int b = a;              // b 是变量，也是一个左值。a 是有名称的，也是一个左值
b = (a + 1);            // (a + 1) 是一个右值，它是一个背后的没有名称的值
b = getValue();         // getValue() 的返回值是一个右值，他没有名称
```
右值引用的意义
直观意义：为临时变量续命，也就是为右值续命，因为右值在表达式结束后就消亡了，如果想继续使用右值，那就会动用昂贵的拷贝构造函数。（关于这部分，推荐一本书《深入理解C++11》）
右值引用是用来支持转移语义的。转移语义可以将资源 ( 堆，系统对象等 ) 从一个对象转移到另一个对象，这样能够减少不必要的临时对象的创建、拷贝以及销毁，能够大幅度提高 C++ 应用程序的性能。临时对象的维护 ( 创建和销毁 ) 对性能有严重影响。转移语义是和拷贝语义相对的，可以类比文件的剪切与拷贝，当我们将文件从一个目录拷贝到另一个目录时，速度比剪切慢很多。通过转移语义，临时对象中的资源能够转移其它的对象里。在现有的 C++ 机制中，我们可以定义拷贝构造函数和赋值函数。要实现转移语义，需要定义转移构造函数，还可以定义转移赋值操作符。对于右值的拷贝和赋值会调用转移构造函数和转移赋值操作符。如果转移构造函数和转移拷贝操作符没有定义，那么就遵循现有的机制，拷贝构造函数和赋值操作符会被调用。普通的函数和操作符也可以利用右值引用操作符实现转移语义。
其中std::move能够很好的实现左值转换为右值操作。
6、内联名字空间inline namespace
对嵌套namespace支持内联，当内层namespace为内联时，访问其中的成员可以不使用namespace名字。
```cpp
namespace A
{
    inline namespace B
    {
        int num;
    }
}
A::num = 100;
```
## C++常见问题
1. 变量声明和定义区别？

    - 声明仅仅是把变量的声明的位置及类型提供给编译器，并不分配内存空间；定义要在定义的地方为其分配存储空间。
   
    - 相同变量可以再多处声明（外部变量extern），但只能在一处定义。

2. "零值比较"？
    - bool类型：if(flag)
    
    - int类型：if(flag == 0)
    
    - 指针类型：if(flag == null)
    
    - float类型：if((flag >= -0.000001) && (flag <= 0. 000001))

3. strlen和sizeof区别？
    - sizeof是运算符，并不是函数，结果在编译时得到而非运行中获得；strlen是字符处理的库函数。
    
    - sizeof参数可以是任何数据的类型或者数据（sizeof参数不退化）；strlen的参数只能是字符指针且结尾是'\0'的字符串。

    - **因为sizeof值在编译时确定，所以不能用来得到动态分配（运行时分配）存储空间的大小。**

4. 同一不同对象可以互相赋值吗？
    - 可以，但含有指针成员时需要注意。
    
    - 对比类的对象赋值时深拷贝和浅拷贝。

5. 结构体内存对齐问题？
    - 结构体内成员按照声明顺序存储，第一个成员地址和整个结构体地址相同。
    
    - 未特殊说明时，按结构体中size最大的成员对齐（若有double成员），按8字节对齐。


6. malloc和new的区别？
    - malloc和free是标准库函数，支持覆盖；new和delete是运算符，并且支持重载。
    
    - malloc仅仅分配内存空间，free仅仅回收空间，不具备调用构造函数和析构函数功能，用malloc分配空间存储类的对象存在风险；new和delete除了分配回收功能外，还会调用构造函数和析构函数。
    
    - malloc和free返回的是void类型指针（必须进行类型转换），new和delete返回的是具体类型指针。

7. 指针和引用区别？
    - 引用只是别名，不占用具体存储空间，只有声明没有定义；指针是具体变量，需要占用存储空间。
    
    - 引用在声明时必须初始化为另一变量，一旦出现必须为typename refname &varname形式；指针声明和定义可以分开，可以先只声明指针变量而不初始化，等用到时再指向具体变量。
    
    - 引用一旦初始化之后就不可以再改变（变量可以被引用为多次，但引用只能作为一个变量引用）；指针变量可以重新指向别的变量。
    
    - 不存在指向空值的引用，必须有具体实体；但是存在指向空值的指针。

8. 宏定义和函数有何区别？
    - 宏在编译时完成替换，之后被替换的文本参与编译，相当于直接插入了代码，运行时不存在函数调用，执行起来更快；函数调用在运行时需要跳转到具体调用函数。
    
    - 宏函数属于在结构中插入代码，没有返回值；函数调用具有返回值。
    
    - 宏函数参数没有类型，不进行类型检查；函数参数具有类型，需要检查类型。
    
    - 宏函数不要在最后加分号。

9. 宏定义和const区别？
    - 宏替换发生在编译阶段之前，属于文本插入替换；const作用发生于编译过程中。
    
    - 宏不检查类型；const会检查数据类型。
    
    - 宏定义的数据没有分配内存空间，只是插入替换掉；const定义的变量只是值不能改变，但要分配内存空间。

10. 宏定义和typedef区别？
    - 宏主要用于定义常量及书写复杂的内容；typedef主要用于定义类型别名。
    
    - 宏替换发生在编译阶段之前，属于文本插入替换；typedef是编译的一部分。
    
    - 宏不检查类型；typedef会检查数据类型。
    
    - 宏不是语句，不在在最后加分号；typedef是语句，要加分号标识结束。
    
    - 注意对指针的操作，typedef char * p_char和#define p_char char *区别巨大。

11. 宏定义和内联函数(inline)区别？
    - 在使用时，宏只做简单字符串替换（编译前）。而内联函数可以进行参数类型检查（编译时），且具有返回值。
    
    - 内联函数本身是函数，强调函数特性，具有重载等功能。
    
    - 内联函数可以作为某个类的成员函数，这样可以使用类的保护成员和私有成员。而当一个表达式涉及到类保护成员或私有成员时，宏就不能实现了。

12. 条件编译#ifdef, #else, #endif作用？
    - 可以通过加#define，并通过#ifdef来判断，将某些具体模块包括进要编译的内容。
    
    - 用于子程序前加#define DEBUG用于程序调试。
    
    - 应对硬件的设置（机器类型等）。
    
    - 条件编译功能if也可实现，但条件编译可以减少被编译语句，从而减少目标程序大小。

13. 区别以下几种变量？

        const int a;
        int const a;
        const int *a;
        int *const a;

    - int const a和const int a均表示定义常量类型a。
    
    - const int *a，其中a为指向int型变量的指针，const在 * 左侧，表示a指向不可变常量。(看成const (*a)，对引用加const)
    
    - int *const a，依旧是指针类型，表示a为指向整型数据的常指针。(看成const(a)，对指针const)

14. volatile有什么作用？
    - volatile定义变量的值是易变的，每次用到这个变量的值的时候都要去重新读取这个变量的值，而不是读寄存器内的备份。
    
    - 多线程中被几个任务共享的变量需要定义为volatile类型。

15. 什么是常引用？
    - 常引用可以理解为常量指针，形式为const typename & refname = varname。
    
    - 常引用下，原变量值不会被别名所修改。
    
    - 原变量的值可以通过原名修改。
    
    - 常引用通常用作只读变量别名或是形参传递。

16. 区别以下指针类型？

        int *p[10]
        int (*p)[10]
        int *p(int)
        int (*p)(int)

    - int *p[10]表示指针数组，强调数组概念，是一个数组变量，数组大小为10，数组内每个元素都是指向int类型的指针变量。
    
    - int (*p)[10]表示数组指针，强调是指针，只有一个变量，是指针类型，不过指向的是一个int类型的数组，这个数组大小是10。
    
    - int *p(int)是函数声明，函数名是p，参数是int类型的，返回值是int *类型的。
    
    - int (*p)(int)是函数指针，强调是指针，该指针指向的函数具有int类型参数，并且返回值是int类型的。

17. 常量指针和指针常量区别？
    - 常量指针是一个指针，读成常量的指针，指向一个只读变量。如int const *p或const int *p。
    
    - 指针常量是一个不能给改变指向的指针。如int *const p。

18. a和&a有什么区别？

        假设数组int a[10];
        int (*p)[10] = &a;

    - a是数组名，是数组首元素地址，+1表示地址值加上一个int类型的大小，如果a的值是0x00000001，加1操作后变为0x00000005。*(a + 1) = a[1]。
    
    - &a是数组的指针，其类型为int (*)[10]（就是前面提到的数组指针），其加1时，系统会认为是数组首地址加上整个数组的偏移（10个int型变量），值为数组a尾元素后一个元素的地址。
    
    - 若(int *)p ，此时输出 *p时，其值为a[0]的值，因为被转为int *类型，解引用时按照int类型大小来读取。

19. 数组名和指针（这里为指向数组首元素的指针）区别？
    - 二者均可通过增减偏移量来访问数组中的元素。

    - 数组名不是真正意义上的指针，可以理解为常指针，所以数组名没有自增、自减等操作。
    
    - 当数组名当做形参传递给调用函数后，就失去了原有特性，退化成一般指针，多了自增、自减操作，但sizeof运算符不能再得到原数组的大小了。

20. 野指针是什么？
    - 也叫空悬指针，不是指向null的指针，是指向垃圾内存的指针。
    
    - 产生原因及解决办法：
         - 指针变量未及时初始化 => 定义指针变量及时初始化，要么置空。
    
         - 指针free或delete之后没有及时置空 => 释放操作后立即置空。

21. 堆和栈的区别？

    - 申请方式不同。

        - 栈由系统自动分配。

        - 堆由程序员手动分配。

    - 申请大小限制不同。

        - 栈顶和栈底是之前预设好的，大小固定，可以通过ulimit -a查看，由ulimit -s修改。

        - 堆向高地址扩展，是不连续的内存区域，大小可以灵活调整。

    - 申请效率不同。

        - 栈由系统分配，速度快，不会有碎片。

        - 堆由程序员分配，速度慢，且会有碎片。

22. delete和delete[]区别？

    - delete只会调用一次析构函数。

    - delete[]会调用数组中每个元素的析构函数。


#### <span id = "oop">面向对象基础</span>

能够准确理解下面这些问题是从C程序员向C++程序员进阶的基础。当然了，这只是一部分。

1. 面向对象三大特性？

    - 封装性：数据和代码捆绑在一起，避免外界干扰和不确定性访问。
    
    - 继承性：让某种类型对象获得另一个类型对象的属性和方法。
    
    - 多态性：同一事物表现出不同事物的能力，即向不同对象发送同一消息，不同的对象在接收时会产生不同的行为（重载实现编译时多态，虚函数实现运行时多态）。

2. public/protected/private的区别？
    - public的变量和函数在类的内部外部都可以访问。
    
    - protected的变量和函数只能在类的内部和其派生类中访问。
    
    - private修饰的元素只能在类内访问。

3. 对象存储空间？
    - 非静态成员的数据类型大小之和。
    
    - 编译器加入的额外成员变量（如指向虚函数表的指针）。
    
    - 为了边缘对齐优化加入的padding。

4. C++空类有哪些成员函数?
    - 首先，空类大小为1字节。
    
    - 默认函数有：
        - 构造函数
    
        - 析构函数
    
        - 拷贝构造函数
    
        - 赋值运算符

5. 构造函数能否为虚函数，析构函数呢？
    - 析构函数：
        - 析构函数可以为虚函数，并且一般情况下基类析构函数要定义为虚函数。
    
        - 只有在基类析构函数定义为虚函数时，调用操作符delete销毁指向对象的基类指针时，才能准确调用派生类的析构函数（从该级向上按序调用虚函数），才能准确销毁数据。
    
        - 析构函数可以是纯虚函数，含有纯虚函数的类是抽象类，此时不能被实例化。但派生类中可以根据自身需求重新改写基类中的纯虚函数。
    
    - 构造函数：
        - 构造函数不能定义为虚函数。在构造函数中可以调用虚函数，不过此时调用的是正在构造的类中的虚函数，而不是子类的虚函数，因为此时子类尚未构造好。

6. 构造函数调用顺序，析构函数呢？
    - 调用所有虚基类的构造函数，顺序为从左到右，从最深到最浅

    - 基类的构造函数：如果有多个基类，先调用纵向上最上层基类构造函数，如果横向继承了多个类，调用顺序为派生表从左到右顺序。
    
    - 如果该对象需要虚函数指针(vptr)，则该指针会被设置从而指向对应的虚函数表(vtbl)。
    
    - 成员类对象的构造函数：如果类的变量中包含其他类（类的组合），需要在调用本类构造函数前先调用成员类对象的构造函数，调用顺序遵照在类中被声明的顺序。
    
    - 派生类的构造函数。
    
    - 析构函数与之相反。

7. 拷贝构造函数中深拷贝和浅拷贝区别？
    - 深拷贝时，当被拷贝对象存在动态分配的存储空间时，需要先动态申请一块存储空间，然后逐字节拷贝内容。
    
    - 浅拷贝仅仅是拷贝指针字面值。
    
    - 当使用浅拷贝时，如果原来的对象调用析构函数释放掉指针所指向的数据，则会产生空悬指针。因为所指向的内存空间已经被释放了。

8. 拷贝构造函数和赋值运算符重载的区别？
    - 拷贝构造函数是函数，赋值运算符是运算符重载。
    
    - 拷贝构造函数会生成新的类对象，赋值运算符不能。
    
    - 拷贝构造函数是直接构造一个新的类对象，所以在初始化对象前不需要检查源对象和新建对象是否相同；赋值运算符需要上述操作并提供两套不同的复制策略，另外赋值运算符中如果原来的对象有内存分配则需要先把内存释放掉。
    
    - 形参传递是调用拷贝构造函数（调用的被赋值对象的拷贝构造函数），但并不是所有出现"="的地方都是使用赋值运算符，如下：

            Student s;
            Student s1 = s;    // 调用拷贝构造函数
            Student s2;
            s2 = s;    // 赋值运算符操作

    **注：类中有指针变量时要重写析构函数、拷贝构造函数和赋值运算符**

9. 虚函数和纯虚函数区别？
    - 虚函数是为了实现动态编联产生的，目的是通过基类类型的指针指向不同对象时，自动调用相应的、和基类同名的函数（使用同一种调用形式，既能调用派生类又能调用基类的同名函数）。虚函数需要在基类中加上virtual修饰符修饰，因为virtual会被隐式继承，所以子类中相同函数都是虚函数。当一个成员函数被声明为虚函数之后，其派生类中同名函数自动成为虚函数，在派生类中重新定义此函数时要求函数名、返回值类型、参数个数和类型全部与基类函数相同。
    
    - 纯虚函数只是相当于一个接口名，但含有纯虚函数的类不能够实例化。

10. 覆盖、重载和隐藏的区别？
     - 覆盖是派生类中重新定义的函数，其函数名、参数列表（个数、类型和顺序）、返回值类型和父类完全相同，只有函数体有区别。派生类虽然继承了基类的同名函数，但用派生类对象调用该函数时会根据对象类型调用相应的函数。覆盖只能发生在类的成员函数中。
    
     - 隐藏是指派生类函数屏蔽了与其同名的函数，这里仅要求基类和派生类函数同名即可。其他状态同覆盖。可以说隐藏比覆盖涵盖的范围更宽泛，毕竟参数不加限定。
    
     - 重载是具有相同函数名但参数列表不同（个数、类型或顺序）的两个函数（不关心返回值），当调用函数时根据传递的参数列表来确定具体调用哪个函数。重载可以是同一个类的成员函数也可以是类外函数。

11. 在main执行之前执行的代码可能是什么？

    - 全局对象的构造函数。

12. 哪几种情况必须用到初始化成员列表？

    - 初始化一个const成员。

    - 初始化一个reference成员。

    - 调用一个基类的构造函数，而该函数有一组参数。

    - 调用一个数据成员对象的构造函数，而该函数有一组参数。

14. 重载和函数模板的区别？

    - 重载需要多个函数，这些函数彼此之间函数名相同，但参数列表中参数数量和类型不同。在区分各个重载函数时我们并不关心函数体。

    - 模板函数是一个通用函数，函数的类型和形参不直接指定而用虚拟类型来代表。但只适用于参个数相同而类型不同的函数。

15. this指针是什么？

    - this指针是类的指针，指向对象的首地址。

    - this指针只能在成员函数中使用，在全局函数、静态成员函数中都不能用this。

    - this指针只有在成员函数中才有定义，且存储位置会因编译器不同有不同存储位置。

16. 类模板是什么？

    - 用于解决多个功能相同、数据类型不同的类需要重复定义的问题。

    - 在建立类时候使用template及任意类型标识符T，之后在建立类对象时，会指定实际的类型，这样才会是一个实际的对象。

    - 类模板是对一批仅数据成员类型不同的类的抽象，只要为这一批类创建一个类模板，即给出一套程序代码，就可以用来生成具体的类。

17. 构造函数和析构函数调用时机？

    - 全局范围中的对象：构造函数在所有函数调用之前执行，在主函数执行完调用析构函数。

    - 局部自动对象：建立对象时调用构造函数，离开作用域时调用析构函数。

    - 动态分配的对象：建立对象时调用构造函数，调用释放时调用析构函数。

    - 静态局部变量对象：建立时调用一次构造函数，主函数结束时调用析构函数。

---

# 编译

**编译**

预处理

- 展开所有的宏定义，完成字符常量替换。

- 处理条件编译语句，通过是否具有某个宏来决定过滤掉哪些代码。

- 处理#include指令，将被包含的文件插入到该指令所在位置。

- 过滤掉所有注释语句。

- 添加行号和文件名标识。

- 保留所有#pragma编译器指令。

编译

- 词法分析。

- 语法分析。

- 语义分析。

- 中间语言生成。

- 目标代码生成与优化。

链接

各个源代码模块独立的被编译，然后将他们组装起来成为一个整体，组装的过程就是链接。被链接的各个部分本本身就是二进制文件，所以在被链接时需要将所有目标文件的代码段拼接在一起，然后将所有对符号地址的引用加以修正。

- 静态链接

    静态链接最简单的情况就是在编译时和静态库链接在一起成为完整的可执行程序。这里所说的静态库就是对多个目标文件（.o）文件的打包，通常静态链接的包名为lib****.a，静态链接所有被用到的目标文件都会复制到最终生成的可执行目标文件中。这种方式的好处是在运行时，可执行目标文件已经完全装载完毕，只要按指令序执行即可，速度比较快，但缺点也有很多，在讲动态链接时会比较一下。

    既然静态链接是对目标文件的打包，这里介绍些打包命令。

        gcc -c test1.c    // 生成test1.o
        gcc -c test2.c    // 生成test2.c
        ar cr libtest.a test1.o test2.o

    首先编译得到test1.o和test2.o两个目标文件，之后通过ar命令将这两个文件打包为.a文件，文件名格式为lib + 静态库名 + .a后缀。在生成可执行文件需要使用到它的时候只需要在编译时加上即可。需要注意的是，使用静态库时加在最后的名字不是libtest.a，而是l + 静态库名。

        gcc -o main main.c -ltest

- 动态链接

    静态链接发生于编译阶段，加载至内存前已经完整，但缺点是如果多个程序都需要使用某个静态库，则该静态库会在每个程序中都拷贝一份，非常浪费内存资源，所以出现了动态链接的方式来解决这个问题。

    动态链接在形式上倒是和静态链接非常相似，首先也是需要打包，打包成动态库，不过文件名格式为lib + 动态库名 + .so后缀。不过动态库的打包不需要使用ar命令，gcc就可以完成，但要注意在编译时要加上-fPIC选项，打包时加上-shared选项。

        gcc -fPIC -c test1.c 
        gcc -fPIC -c test2.c
        gcc -shared test1.o test2.o -o libtest.so

    使用动态链接的用法也和静态链接相同。

        gcc -o main main.c -ltest

如果仅仅像上面的步骤是没有办法正常使用库的，我们可以通过加-Lpath指定搜索库文件的目录（-L.表示当前目录），默认情况下会到环境变量LD_LIBRARY_PATH指定的目录下搜索库文件，默认情况是/usr/lib，我们可以将库文件拷贝到那个目录下再链接。

比较静态库和动态库我们可以得到二者的优缺点。

- 动态库运行时会先检查内存中是否已经有该库的拷贝，若有则共享拷贝，否则重新加载动态库（C语言的标准库就是动态库）。静态库则是每次在编译阶段都将静态库文件打包进去，当某个库被多次引用到时，内存中会有多份副本，浪费资源。

- 动态库另一个有点就是更新很容易，当库发生变化时，如果接口没变只需要用新的动态库替换掉就可以了。但是如果是静态库的话就需要重新被编译。

- 不过静态库也有优点，主要就是静态库一次性完成了所有内容的绑定，运行时就不必再去考虑链接的问题了，执行效率会稍微高一些。

makefile编写

对于大的工程通常涉及很多头文件和源文件，编译起来很很麻烦，makefile正是为了自动化编译产生的，makefile像是编译说明书，指示编译的步骤和条件，之后被make命令解释。

- 基本规则

        A:B
        (tab)<command>

    其中A是语句最后生成的文件，B是生成A所依赖的文件，比如生成test.o依赖于test.c和test.h，则写成test.o:test.c test.h。接下来一行的开头必须是tab，再往下就是实际的命令了，比如gcc -c test.c -o test.o。

- 变量

    makefile的书写非常像shell脚本，可以在文件中定义"变量名 = 变量值"的形式，之后需要使用这个变量时只需要写一个$符号加上变量名即可，当然，和shell一样，最好用()包裹起语句来。

**链接**

符号解析

- 可重定位目标文件

    对于独立编译的可重定位目标文件，其ELF文件格式包括ELF头（指定文件大小及字节序）、.text（代码段）、.rodata（只读数据区）、.data（已初始化数据区）、.bss（未初始化全局变量）、.symtab（符号表）等，其中链接时最需要关注的就是符号表。每个可重定位目标文件都有一张符号表，它包含该模块定义和引用的符号的信息，简而言之就是我们在每个模块中定义和引用的全局变量（包括定义在本模块的全局变量、静态全局变量和引用自定义在其他模块的全局变量）需要通过一张表来记录，在链接时通过查表将各个独立的目标文件合并成一个完整的可执行文件。

- 解析符号表

    解析符号引用的目的是将每个引用与可重定位目标文件的符号表中的一个符号定义联系起来。

重定位

- 合并节

    多个可重定位目标文件中相同的节合并成一个完整的聚合节，比如多个目标文件的.data节合并成可执行文件的.data节。链接器将运行时存储地址赋予每个节，完成这步每条指令和全局变量都有运行时地址了。

- 重定位符号引用

    这步修改全部代码节和数据节对每个符号的符号引用，使其指向正确的运行时地址。局部变量可以通过进栈、出栈临时分配，但全局变量（"符号"）的位置则是在各个可重定位目标文件中预留好的。通过上一步合并节操作后，指令中所有涉及符号的引用都会通过一定的寻址方式来定位该符号，比如相对寻址、绝对寻址等。

可执行目标文件

- ELF头部

    描述文件总体格式，并且包括程序的入口点（entry point），也就是程序运行时执行的第一条指令地址。

- 段头部表

    描述了可执行文件数据段、代码段等各段的大小、虚拟地址、段对齐、执行权限等。实际上通过段头部表描绘了虚拟存储器运行时存储映像，比如每个UNIX程序的代码段总是从虚拟地址Ox0804800开始的。

- 其他段

    和可重定位目标文件各段基本相同，但完成了多个节的合并和重定位工作。

加载

- 克隆

    新程序的执行首先需要通过父进程外壳通过fork得到一个子进程，该子进程除了pid等标识和父进程不同外其他基本均与父进程相同。

- 重新映射

    当子进程执行execve系统调用时会先清空子进程现有的虚拟存储器段（简而言之就是不再映射到父进程的各个段），之后重新创建子进程虚拟存储器各段和可执行目标文件各段的映射。这个阶段我们可以理解为对复制来的父进程页表进程重写，映射到外存中可执行文件的各个段。

- 虚页调入

    加载过程并没有实际将磁盘中可执行文件调入内存，所做的工作紧紧是复制父进程页表、清空旧页表、建立新页表映射工作。之后加载器跳转到入口地址_start开始执行程序，接下来的过程需要配合虚拟存储器来完成。CPU获得指令的虚拟地址后，若包含该指令或数据的页尚未调入内存则将其从外存中调入，调入内存后修改页表得到虚拟页号和物理页号的对应关系。之后重新取同一条指令或数据时因该页已经被调入内存，所以通过虚拟地址得到虚拟页号，虚拟页号通过查页表可以得到物理页号，通过物理页号 + 页内偏移得到具体的物理地址，此时可以通过物理地址取得想要的数据。

## ELF文件
ELF是Executable and Linking Format的缩写，即可执行链接格式，最初由UNIX实验室开发发布的，作为应用程序二进制接口的一部分。简单的来说，ELF就是描述了可执行文件的格式规范，也就是说我们的执行文件到底该是什么样的格式。

在理解具体的文件格式之前，我们需要知道ELF文件格式可以用来描述三种类型的文件:

可执行文件：这个就很熟悉了，经常运行的程序便是，这个文件规定了exec()如何来创建一个程序的进程映像。

可重定位文件：包含适合于与其他文件链接来创建可执行文件或者共享目标文件的这么一个文件。

共享目标文件：就是我们常提到的静态库或者动态库了。链接编辑器可以将此文件和其他可重定位文件和共享目标文件一起处理生成另外的一个文件，另外，动态链接器可能将它与某个可执行文件以及其他共享目标一起组合，创建进程映像。