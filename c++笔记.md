笔记呢只简要记录了老师讲课的关键知识点和结构，然后积累了一些有用的代码小片段和解释性很强的例子，之所以没有分章节是为了使用浏览器页内查找(ctrl+f/ command+f)时方便。
课程的编程作业的代码我也上传了gayhub(在这里呀https://github.com/BMR731/XueTangCplusplus).
有错误的地方希望大家指出啦，有一起继续学习C++的小伙伴也可以一起交流哦~



- auto 变量类型，如
```
auto i = j+k;
vector<int> v(5);
for(auto e : v){
    cout<<e;
}
```
- 控制台传参数
```
int main(int argc, char* argv[]){
//argc是参数的个数，包括程序执行本身的一个参数；argv是一个字符串的数组
}
```
- decltype 的使用，```decltype(i) j = 2;//声明一个与i同类型的j```
- C++风格的安全类型转换，如 `int j = static_cast<int>(i);`
- dynamic_cast的转换，dynamic_cast主要用于类层次间的上行转换和下行转换，还可以用于类之间的交叉转换。在类层次间进行上行转换时，dynamic_cast和[static_cast]的效果是一样的；
在进行下行转换时，dynamic_cast具有类型检查的功能，比static_cast更安全。
下面是类型转换时有用的代码片段：
```
if(Derived* dp= dynamic_cast<Derived*)(bp)){
//转换成功，dp指向Derived对象
}else{
//转换失败，bp指向Base对象，
}
//把转换语句写在条件判断中更加安全
```

- char to int 的简单转换
```
int char2int(char c){
    return static_cast<int>(c) - 48;
}
```
- 保留小数点后两位
```
float f = 1.234;
cout<<fixed<<setprecision(2)<<f<<endl;
```
- 反转一个正整数的代码段 
```
unsigned i = n;//n is the number we try to reverse
int m=0;//m is the auxiliary number
while(i>0){
    m= m*10 + i%10;
    i  = i/10;
}
//m is the reversed n;
```
- 随机数的使用
```
#include <cstdlib>
cin>>seed;
srand(seed);
int i = rand()%6 + 1;//模拟扔骰子
```
- 【todo】可变参数传递问题
- 内联函数: 声明时使用关键字 inline。编译时在调用处用函数体进行替换，节省了参数传递、控制转移等开销。
>注意：
>内联函数体内不能有循环语句和switch语句；
>内联函数的定义必须出现在内联函数第一次被调用之前；
>对内联函数不能进行异常接口声明。
```
inline double calArea(int r){//计算圆面积
    return PI*r*r;
}
```
- 传递引用有两个优点:
     - 为了实现双向修改
     - 为了节省传递的开销，但又不允许修改源数据，如复制构造函数` foo(const class &A){...}`,添加const关键词即可
- constexpr函数：constexpr修饰的函数在其所有参数都是constexpr时，一定返回constexpr；并且只有一条return语句；好处是表达式的值可以在编译时进行确定。
```
constexpr int get_size(){ return 20;}
constexpr int foo = get_size();//此时可以确保foo是一个常量表达式
```
- 默认参数值：
```
int add(int x, int y=2, int z=3);//对的，默认参数给的顺序必须从右到左
int add(int x =1, int y, int z=3);//错
```
- 函数重载：注意一下，返回值和形参类型不能区分重载函数即可
- 在自定义构造函数后仍希望编译器给出默认参数的话,可这样写
`clock() = default;`
- 用初始化列表的方式赋值更快
```
Clock::Clock(int newH,int newM,int newS): hour(newH),minute(newM),  second(newS) {
  }
```
- 设计类时，写一个的默认构造函数是良好的设计规范。
- 委托构造函数就是可以在构造函数中调用其他构造函数的机制
- 复制构造函数
```
class 类名 {
public :
    类名（形参）；//构造函数
    类名（const  类名 &对象名）；//复制构造函数
    //       ...
}；

类名::类（ const  类名 &对象名）//复制构造函数的实现
{    函数体    }

//如果不希望被复制
//class Point {   //Point 类的定义
public:
    Point(int xx=0, int yy=0) { x = xx; y = yy; }    //构造函数，内联
    Point(const Point& p) =delete;  //指示编译器不生成默认复制构造函数
private:
    int x, y; //私有数据
};

//复制构造函数被调用的三种时机
//1  用一个对象初始化对象时
//2  形参和实参结合时
//3  return 语句返回一个无名对象时
```
- 析构函数
完成对象被删除前的一些清理工作。
在对象的生存期结束的时刻系统自动调用它，然后再释放此对象所属的空间。
如果程序中未声明析构函数，编译器将自动产生一个默认的析构函数，其函数体为空。
```构造函数和析构函数举例
class Point {     
public:
  Point(int xx,int yy);
  ~Point();
  //...其他函数原型
private:
  int x, y;
};
```
- 前向引用申明：为了解决两个类在定义时相互引用的情况，但又不能完美解决所有情况，如它可以解决充当形参的情况，但不能解决充当成员变量的情况，因为涉及到具体的字节数等细节问题。
```
class B;  //前向引用声明
class A {
public:
  void f(B b);
};

class B {
public:
  void g(A a);
};
```
- 类的静态成员别忘了在类外进行定义和初始化
```
class foo{
  private:
    static int count;
}
int foo::count =0;//this line don't forget;在类外进行定义和初始化！
int main(){}
```
-   结构体;结构体是一种特殊形态的类
与类的唯一区别：类的缺省访问权限是private，结构体的缺省访问权限是public
结构体存在的主要原因：与C语言保持兼容
什么时候用结构体而不用类呢?定义主要用来保存数据、而没有什么操作的类型；人们习惯将结构体的数据成员设为公有，因此这时用结构体更方便
- 联合体：目的是存储空间的共用，减少冗余和错误。
- 枚举类：实质上就是强类型的枚举，与简单枚举相比，防止冲突；类型要求严格；更加多样的基本类型，
```
enum class Type: char { General, Light, Medium, Heavy};
```
- 类的友元：友元机制是破坏封装的一种机制，为的是提供封装和效率的折中，在水平不高时最好少使用；友元是一种单向的关系；
```
//友元函数
class Point { //Point类声明
public: //外部接口
  Point(int x=0, int y=0) : x(x), y(y) { }
    int getX() { return x; }
    int getY() { return y; }
    friend float dist(Point &a, Point &b);
private: //私有数据成员
    int x, y;
};

float dist( Point& a, Point& b) {
  double x = a.x - b.x;
  double y = a.y - b.y;
  return static_cast<float>(sqrt(x * x + y * y));
}

//友元类
class A {
friend class B;
public:
void display() {
cout << x << endl;
}

private:
int x;
};

class B {
public:
void set(int i);
void display();
private:
A a;
};

void B::set(int i) {
a.x=i;
}

void B::display() {
a.display();
};
```
- const的用法
  - 常函数`void A::print() const;`对于保证不改变对象状态的函数，优先声明const来得到编译器的保证检查。
  - 常变量`const int a = 2;`
  - 常引用：用于传引用但保证单向传递的情况，` void foo(const int &a)`
- 多文件结构
  - .h文件：类的声明
  - .cpp文件： 类的实现
  - main()所在文件：类的使用文件
- 预编译指令：
```
#include
#define
#if....#endif 条件编译
#if..#elif....#else...#endif
#ifdef.. #endif 如果标记被定义过
#ifndef....#endif 如果标记未被定义过

最常用的用法在类的声明文件中，为了避免重复编译，常这样写：
#ifndef CLIENT_H
#define CLIENT_H
...类的声明
#endif
```
- 指针相关
```
const int* p = &i; //表明p为只读指针，但指针本身可以指向其他地方
int* const p = &i;//表明指针本身只能指向i的地址，但可对i进行读写操作

//void指针作为通用指针来使用
void* p;
int i=0;
p = &i;
int* p2 = static_cast<int*>(p);

//空指针；
int* p = nullptr; //c++11 推荐
p==0;//判断指针是否为空
```

- 指针做函数参数，为什么要用指针
需要数据双向传递时（引用也可以达到此效果）
需要传递一组数据，只传首地址运行效率比较高

- 返回指针类型的函数，1.特别注意返回的地址不能是局部变量的地址，必须在主调函数中有效。2.函数返回用new分配的空间分配的地址是可以的，但主调函数必须记得释放空间

- 函数指针。函数指针的主要用途是实现函数回调，从而调用者可以将函数作为参数，更加灵活的处理数据。函数指针与其他类型的指针声明相同，只不过要求更多一些，需要表明函数的返回值，参数表，如`int(*func)(int, int)`表明这是一个指向返回值类型为int，参数表为(int,int)的函数的程序代码的地址。
```

int compute(int a, int b,  int(*func)(int,int)){
	return func(a,b);
}

int max(int a, int b){return ((a>b)? a:b;)}
int min(int a, int b){return ((a<b)? a:b;)}

res = compute(a,b,&max);
res = compute(a,b,&min);
```
- 对象指针，1. 了解`pa->getX()与(*pa).getX()`等价 2.了解this指针
- 动态分配内存
```
//分配多维数组
int (*cp)[8][9] = new int[7][8][9];
```
- 左值和右值的问题，实质上是能不能修改的问题，右值表示只可以读但不可以修改，而左值才能修改，返回左值的常见做法是返回引用类型或指针类型。

- 智能指针[Todo有待补充理解]
unique_ptr ：不允许多个指针共享资源，可以用标准库中的move函数转移指针
shared_ptr ：多个指针共享资源
weak_ptr ：可复制shared_ptr，但其构造或者释放对资源不产生影响
- 深层复制和浅层复制
当类成员为指针变量时，如一个数组，浅层复制只是复制了指针的值，而深层复制才可以复制指针所指的内容，此时多需要重写复制拷贝函数
- 移动构造函数：在一些情况下，不需要复制构造时，而只需简单移动，将控制权转移给目标对象时，可使用移动构造函数，书写格式为`class_name ( class_name && )` &&表示右值引用。
- 求字符串所有子序列
```
vector<string> get_subsequences(const string str){
    long len = str.length();
    long num = 1<<str.length();//将1左移len位，求2的len次幂。
    vector<string> res;
    for (int i = 1; i <num ; ++i) {
        string ss;
        for (int j = 0; j < len; ++j) {
            if(i&(1<<j)) ss.push_back(str[j]);
        }
        res.push_back(ss);
    }
    return res;
}
```
- vector的使用
```
vector<int> nums(5,2);//初始化
sort(nums.begin(), nums.end());//排序
```
- 派生的继承方式：
 **公有继承(public)**
继承的访问控制：
基类的public和protected成员：访问属性在派生类中保持不变；
基类的private成员：不可直接访问。
访问权限：
派生类中的成员函数：可以直接访问基类中的public和protected成员，但不能直接访问基类的private成员；
通过派生类的对象：只能访问public成员。
**私有继承**
继承的访问控制
基类的public和protected成员：都以private身份出现在派生类中；
基类的private成员：不可直接访问。
访问权限
派生类中的成员函数：可以直接访问基类中的public和protected成员，但不能直接访问基类的private成员；
通过派生类的对象：不能直接访问从基类继承的任何成员。
**保护继承(protected)**
继承的访问控制
基类的public和protected成员：都以protected身份出现在派生类中；
基类的private成员：不可直接访问。
访问权限
派生类中的成员函数：可以直接访问基类中的public和protected成员，但不能直接访问基类的private成员；
通过派生类的对象：不能直接访问从基类继承的任何成员。
protected 成员的特点与作用
对建立其所在类对象的模块来说，它与 private 成员的性质相同。
对于其派生类来说，它与 public 成员的性质相同。
既实现了数据隐藏，又方便继承，实现代码重用。
如果派生类有多个基类，也就是多继承时，可以用不同的方式继承每个基类。

- 基类和私有类之间的类型转换
公有派生类对象可以被当作基类的对象使用，反之则不可。
派生类的对象可以隐含转换为基类对象；
派生类的对象可以初始化基类的引用；
派生类的指针可以隐含转换为基类的指针。
通过基类对象名、指针只能使用从基类继承的成员。

- 派生类的构造函数：首先，按照继承的次序，确保给基类带参数的初始化函数送去参数，接着按照类成员的声明次序初始化类成员，最后调用构造函数。
```
class C: public B {
public:
    C();
    C(int i, int j);
    ~C();
    void print() const;
private:
    int c;
};
C::C(int i,int j): B(i), c(j){
    cout << "C's constructor called." << endl;
}
```

- 派生类的复制构造函数
一般都要为基类的复制构造函数传递参数。
复制构造函数只能接受一个参数，既用来初始化派生类定义的成员，也将被传递给基类的复制构造函数。
基类的复制构造函数形参类型是基类对象的引用，实参可以是派生类对象的引用
例如: `C::C(const C &c1): B(c1) {…}`

- 派生类的析构函数：无需显示调用，析构次序与初始化次序相反。

- 二义性：二义性可以发生在父子之间，父亲之间，简单的解决方案是使用类名加以限定即可，然而在多继承下的二义性问题比较复杂，因此引入了虚基类，当多个父亲有一个共同的祖先时，祖先里的成员存在不一致性和冗余的风险，因此通过虚继承来保证祖先中的值只有一份，并且祖先的构造函数需要在每一代的构造函数中传参，但实质上执行的只有最远派生类调用的构造函数。

- C++中class声明的默认权限是private，struct声明的默认权限是public

- 重载运算符，重载函数可以重载为类内成员函数，或者类外函数。

重载为类内成员函数的要求是，操作符的第一个参数必须是该类的类型。
```
//双目运算符的重载
//例8-1复数类加减法运算重载为成员函数
Complex Complex::operator + (const Complex &c2) const{
  //创建一个临时无名对象作为返回值 
  return Complex(real+c2.real, imag+c2.imag); 
}
//单目运算符的重载
//重载前置++
Clock & Clock::operator ++ () { 
    second++;
    if (second >= 60) {
        second -= 60;  minute++;
        if (minute >= 60) {
          minute -= 60; hour = (hour + 1) % 24;
        }
    }
    return *this;//返回值的本身引用，可以当左值被修改
}
//重载后置++
Clock Clock::operator ++ (int) {
    //注意形参表中的整型参数
    Clock old = *this;
    ++(*this);  //调用前置“++”运算符
    return old;//返回值的一个副本，只能做右值，不能做触及到本身的修改
}
```

重载为非成员函数的规则
函数的形参代表依自左至右次序排列的各操作数。
重载为非成员函数时，参数个数=原操作数个数（后置++、--除外）
至少应该有一个自定义类型的参数。
后置单目运算符 ++和--的重载函数，形参列表中要增加一个int，但不必写形参名。
如果在运算符的重载函数中需要操作某类对象的私有成员，可以将此函数声明为该类的友元。
典型例题：
```
 class Complex {
    public:
        Complex(double r = 0.0, double i = 0.0) : real(r), imag(i) { }  
        friend Complex operator+(const Complex &c1, const Complex &c2);//声明类外的函数为友元来提高访问的效率
        friend Complex operator-(const Complex &c1, const Complex &c2);
        friend ostream & operator<<(ostream &out, const Complex &c);
    private:    
        double real;  //复数实部
        double imag;  //复数虚部
    };
    Complex operator+(const Complex &c1, const Complex &c2){//注意这里传const的引用来提高传输效率
        return Complex(c1.real+c2.real, c1.imag+c2.imag); 
    }
    Complex operator-(const Complex &c1, const Complex &c2){
        return Complex(c1.real-c2.real, c1.imag-c2.imag); 
    }

    ostream & operator<<(ostream &out, const Complex &c){
        out << "(" << c.real << ", " << c.imag << ")";
        return out;//返回为ostream的引用来继续保持cout的级联输出
    }

```
- 虚函数：
  - 虚函数是实现动态绑定的一种机制，告诉编译器运行时绑定从而实现多态
  - 虚函数同时不可以成为内联函数，必须在类外定义
  - 基类只要是虚函数，则它子类的相同函数也一定是虚函数，不过我们也要显示的声明来增加可读性
  - 在继承时，不要重写父类的非虚函数，否则静态绑定后全都会使用父类的函数版本。
  -  `virtual void print() const`

- 虚析构函数：没有虚构造函数，但是有虚析构函数，为什么需要虚析构函数？ 可能通过基类指针删除派生类对象； 如果你打算允许其他人通过基类指针调用对象的析构函数（通过delete这样做是正常的），就需要让基类的析构函数成为虚函数，否则执行delete的结果是不确定的。

- 抽象类：多用作基类用于规范接口，只要有一个纯虚函数的类就是抽象类，不能实例化，其中，纯虚函数语法为` virtual void print() const = 0;`

- override和final的使用，override的使用可以使编译器在编译时进行检查，以免发生难以调试的运行时错误，要习惯使用。
```
struct B4
{
    virtual void g(int) {}
};

struct D4 : B4
{
    virtual void g(int) override {} // OK
    virtual void g(double) override {} // Error
};
```
```
struct B2
{
    virtual void f() final {} // final 函数
};

struct D2 : B2
{
    virtual void f() {}
};
```

- 注意区分虚基类和虚函数的作用，虚基类是为了消除多继承中的二义性而引入的，而虚函数是为了实现多态性而引入的。
- 模板：编译器帮我们的一种机制，使用时注意若要操作自定义类型时，请确保在类内重载了相应的运算符。
```
//函数模板
template <typename T>
T add(T x, T y){
    return x+y;
}
//类模板
template <class T>
class Foo{
  T element;
   Foo();
}
Foo<T>::Foo(){}//注意此时在类外标注类名时要把模板参数带上,写成Foo<T>::
```
- **术语：概念**
用来界定具备一定功能的数据类型。例如：
将“可以比大小的所有数据类型（有比较运算符）”这一概念记为Comparable
将“具有公有的复制构造函数并可以用‘=’赋值的数据类型”这一概念记为Assignable
将“可以比大小、具有公有的复制构造函数并可以用‘=’赋值的所有数据类型”这个概念记作Sortable
对于两个不同的概念A和B，如果概念A所需求的所有功能也是概念B所需求的功能，那么就说概念B是概念A的子概念。例如：
Sortable既是Comparable的子概念，也是Assignable的子概念。
**术语：模型**
模型（model）：符合一个概念的数据类型称为该概念的模型，例如：
int型是Comparable概念的模型。
静态数组类型不是Assignable概念的模型（无法用“=”给整个静态数组赋值）

- STL：由迭代器，函数对象，容器，算法四部分组成
- 迭代器：从功能上可理解为一个泛型指针，
```
//求平方的函数
double square(double x) {
    return x * x;
}
int main() {
    //从标准输入读入若干个实数，分别将它们的平方输出
    transform(istream_iterator<double>(cin), istream_iterator<double>(),
        ostream_iterator<double>(cout, "\t"), square);
    cout << endl;
    return 0;
}

//程序涉及到输入迭代器、输出迭代器、随机访问迭代器这三个迭代器概念，并且以前两个概念为基础编写了一个通用算法。
#include <algorithm>
#include <iterator>
#include <vector>
#include <iostream>
using namespace std;

//将来自输入迭代器的n个T类型的数值排序，将结果通过输出迭代器result输出
template <class T, class InputIterator, class OutputIterator>
void mySort(InputIterator first, InputIterator last, OutputIterator result) {
    //通过输入迭代器将输入数据存入向量容器s中
    vector<T> s;
    for (;first != last; ++first)
        s.push_back(*first);
    //对s进行排序，sort函数的参数必须是随机访问迭代器
    sort(s.begin(), s.end());  
    copy(s.begin(), s.end(), result);   //将s序列通过输出迭代器输出
}

int main() {
    //将s数组的内容排序后输出
    double a[5] = { 1.2, 2.4, 0.8, 3.3, 3.2 };
    mySort<double>(a, a + 5, ostream_iterator<double>(cout, " "));
    cout << endl;
    //从标准输入读入若干个整数，将排序后的结果输出
    mySort<int>(istream_iterator<int>(cin), istream_iterator<int>(), ostream_iterator<int>(cout, " "));
    cout << endl;
    return 0;
}
/*
```
- 容器：
![容器的分类.png](https://upload-images.jianshu.io/upload_images/3534499-8751334f0fff46a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**容器的通用功能**
用默认构造函数构造空容器
支持关系运算符：==、!=、<、<=、>、>=
begin()、end()：获得容器首、尾迭代器
clear()：将容器清空
empty()：判断容器是否为空
size()：得到容器元素个数
s1.swap(s2)：将s1和s2两容器内容交换
相关数据类型（S表示容器类型）
S::iterator：指向容器元素的迭代器类型
S::const_iterator：常迭代器类型
**可逆容器**
STL为每个可逆容器都提供了逆向迭代器，逆向迭代器可以通过下面的成员函数得到：
rbegin() ：指向容器尾的逆向迭代器
rend()：指向容器首的逆向迭代器
逆向迭代器的类型名的表示方式如下：
S::reverse_iterator：逆向迭代器类型
S::constreverseiterator：逆向常迭代器类
**随机访问容器**
随机访问容器支持对容器的元素进行随机访问
s[n]：获得容器s的第n个元素

- 顺序容器的基本操作
```
#include <iostream>
#include <list>
#include <deque>

//输出指定的顺序容器的元素
template <class T>
void printContainer(const char* msg, const T& s) {
    cout << msg << ": ";
    copy(s.begin(), s.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
}

int main() {
    //从标准输入读入10个整数，将它们分别从s的头部加入
    deque<int> s;
    for (int i = 0; i < 10; i++) {
        int x;
        cin >> x;
        s.push_front(x);
    }
    printContainer("deque at first", s);
    //用s容器的内容的逆序构造列表容器l
    list<int> l(s.rbegin(), s.rend());
    printContainer("list at first", l);

    //将列表容器l的每相邻两个元素顺序颠倒
    list<int>::iterator iter = l.begin();
    while (iter != l.end()) {
        int v = *iter;  
        iter = l.erase(iter);
        l.insert(++iter, v);
    }
    printContainer("list at last", l);
    //用列表容器l的内容给s赋值，将s输出
    s.assign(l.begin(), l.end());
    printContainer("deque at last", s);
    return 0;
}

int main() {
    istream_iterator<int> i1(cin), i2;  //建立一对输入流迭代器
    vector<int> s1(i1, i2); //通过输入流迭代器从标准输入流中输入数据
    sort(s1.begin(), s1.end()); //将输入的整数排序
    deque<int> s2;
    //以下循环遍历s1
    for (vector<int>::iterator iter = s1.begin(); iter != s1.end(); ++iter) 
    {
         if (*iter % 2 == 0)    //偶数放到s2尾部
             s2.push_back(*iter);
         else       //奇数放到s2首部
             s2.push_front(*iter);
    }
    //将s2的结果输出
    copy(s2.begin(), s2.end(), ostream_iterator<int>(cout, " "));
    cout << endl;
    return 0;
}

```

STL所提供的顺序容器各有所长也各有所短，我们在编写程序时应当根据我们对容器所需要执行的操作来决定选择哪一种容器。
如果需要执行大量的随机访问操作，而且当扩展容器时只需要向容器尾部加入新的元素，就应当选择向量容器vector；
如果需要少量的随机访问操作，需要在容器两端插入或删除元素，则应当选择双端队列容器deque；
如果不需要对容器进行随机访问，但是需要在中间位置插入或者删除元素，就应当选择列表容器list或forward_list；
如果需要数组，array相对于内置数组类型而言，是一种更安全、更容易使用的数组类型。
**顺序容器的插入迭代器**
用于向容器头部、尾部或中间指定位置插入元素的迭代器
包括前插迭代器（frontinserter）、后插迭代器（backinsrter）和任意位置插入迭代器（inserter）.

 - 集合
```c++
输入一串实数，将重复的去掉，取最大和最小者的中值，分别输出小于等于此中值和大于等于此中值的实数

//10_9.cpp
#include <set>
#include <iterator>
#include <utility>
#include <iostream>
using namespace std;

int main() {
    set<double> s;
    while (true) {
        double v;
        cin >> v;
        if (v == 0) break;  //输入0表示结束
        //尝试将v插入
       pair<set<double>::iterator,bool> r=s.insert(v); 
        if (!r.second)  //如果v已存在，输出提示信息
           cout << v << " is duplicated" << endl;
    }
  //得到第一个元素的迭代器
    set<double>::iterator iter1=s.begin();
    //得到末尾的迭代器
    set<double>::iterator iter2=s.end();    
  //得到最小和最大元素的中值    
    double medium=(*iter1 + *(--iter2)) / 2;    
    //输出小于或等于中值的元素
    cout<< "<= medium: "
    copy(s.begin(), s.upper_bound(medium), ostream_iterator<double>(cout, " "));
    cout << endl;
    //输出大于或等于中值的元素
    cout << ">= medium: ";
    copy(s.lower_bound(medium), s.end(), ostream_iterator<double>(cout, " "));
    cout << endl;
    return 0;
}
```
- map
```
统计一句话中每个字母出现的次数
// 10_11.cpp
#include <iostream>
#include <map>
#include <cctype>
using namespace std;
int main() {
    map<char, int> s;   //用来存储字母出现次数的映射
    char c;     //存储输入字符
    do {
      cin >> c; //输入下一个字符
      if (isalpha(c)){ //判断是否是字母
          c = tolower(c); //将字母转换为小写
          s[c]++;      //将该字母的出现频率加1
      }
    } while (c != '.'); //碰到“.”则结束输入
    //输出每个字母出现次数
    for (map<char, int>::iterator iter = s.begin(); iter != s.end(); ++iter)
        cout << iter->first << " " << iter->second << "  ";
    cout << endl;
    return 0;
}
```
- multiset 和multimap
```
//10_12.cpp
#include <iostream>
#include <map>
#include <utility>
#include <string>
using namespace std;
int main() {
    multimap<string, string> courses;
    typedef multimap<string, string>::iterator CourseIter;

    //将课程上课时间插入courses映射中
    courses.insert(make_pair("C++", "2-6"));
    courses.insert(make_pair("COMPILER", "3-1"));
    courses.insert(make_pair("COMPILER", "5-2"));
    courses.insert(make_pair("OS", "1-2"));
    courses.insert(make_pair("OS", "4-1"));
    courses.insert(make_pair("OS", "5-5"));
    //输入一个课程名，直到找到该课程为止，记下每周上课次数
    string name;
    int count;
    do {
        cin >> name;
        count = courses.count(name);
        if (count == 0)
          cout << "Cannot find this course!" << endl;
    } while (count == 0);
    //输出每周上课次数和上课时间
    cout << count << " lesson(s) per week: ";
    pair<CourseIter, CourseIter> range = courses.equal_range(name);
    for (CourseIter iter = range.first; iter != range.second; ++iter)
        cout << iter->second << " ";
    cout << endl;

    return 0;
}
```

- 函数对象：
>STL提供的函数对象
>用于算术运算的函数对象：
>一元函数对象(一个参数) ：negate
>二元函数对象(两个参数) ：plus、minus、multiplies、divides、modulus
>用于关系运算、逻辑运算的函数对象(要求返回值为bool)
>一元谓词(一个参数)：logical_not
>二元谓词(两个参数)：equalto、notequalto、greater、less、greaterequal、lessequal、logicaland、logical_or
```
#include <funtional>
sort(a.begin(), a.end(), greater<int>());
cout << accumulate(a, a + N, 1, multiplies<int>());
cout << accumulate(a, a + N, 1, mult)
```
- 函数适配器（感觉很难懂，不知怎么用，把代码全都搞上来了）
  - 绑定适配器：bind1st、bind2nd
将n元函数对象的指定参数绑定为一个常数，得到n-1元函数对象
  - 组合适配器：not1、not2
将指定谓词的结果取反
  - 函数指针适配器：ptr_fun
将一般函数指针转换为函数对象，使之能够作为其它函数适配器的输入。
在进行参数绑定或其他转换的时候，通常需要函数对象的类型信息，例如bind1st和bind2nd要求函数对象必须继承于binary_function类型。但如果传入的是函数指针形式的函数对象，则无法获得函数对象的类型信息。
  - 成员函数适配器：ptrfun、ptrfun_ref
对成员函数指针使用，把n元成员函数适配为n + 1元函数对象，该函数对象的第一个参数为调用该成员函数时的目的对象
也就是需要将“object->method()”转为“method(object)”形式。将“object->method(arg1)”转为二元函数“method(object, arg1)”。
```
//数适配器实例——找到数组中第一个大于40的元素
int main() {
    int intArr[] = { 30, 90, 10, 40, 70, 50, 20, 80 };
    const int N = sizeof(intArr) / sizeof(int);
    vector<int> a(intArr, intArr + N);
    vector<int>::iterator p = find_if(a.begin(), a.end(), bind2nd(greater<int>(), 40));
    if (p == a.end())
        cout << "no element greater than 40" << endl;
    else
        cout << "first element greater than 40 is: " << *p << endl;
    return 0;
}

注：
find_if算法在STL中的原型声明为：
template<class InputIterator, class UnaryPredicate>
InputIterator find_if(InputIterator first, InputIterator last, UnaryPredicate pred);
它的功能是查找数组[first, last)区间中第一个pred(x)为真的元素。

//ptr_fun、not1和not2产生函数适配器实例
bool g(int x, int y) {
    return x > y;
}

int main() {
    int intArr[] = { 30, 90, 10, 40, 70, 50, 20, 80 };
    const int N = sizeof(intArr) / sizeof(int);
    vector<int> a(intArr, intArr + N);
    vector<int>::iterator p;
    p = find_if(a.begin(), a.end(), bind2nd(ptr_fun(g), 40));
    if (p == a.end())
        cout << "no element greater than 40" << endl;
    else
        cout << "first element greater than 40 is: " << *p << endl;
    p = find_if(a.begin(), a.end(), not1(bind2nd(greater<int>(), 15)));
    if (p == a.end())
        cout << "no element is not greater than 15" << endl;
    else
        cout << "first element that is not greater than 15 is: " << *p << endl;

    p = find_if(a.begin(), a.end(), bind2nd(not2(greater<int>()), 15));
    if (p == a.end())
        cout << "no element is not greater than 15" << endl;
    else
        cout << "first element that is not greater than 15 is: " << *p << endl;
    return 0;
}

// 成员函数适配器实例
struct Car {
    int id;
    Car(int id) { this->id = id; }
    void display() const { cout << "car " << id << endl; }
};

int main() {
    vector<Car *> pcars;
    vector<Car> cars;
    for (int i = 0; i < 5; i++)
        pcars.push_back(new Car(i));
    for (int i = 5; i < 10; i++)
        cars.push_back(Car(i));
    cout << "elements in pcars: " << endl;
    for_each(pcars.begin(), pcars.end(), std::mem_fun(&Car::display));
    cout << endl;

    cout << "elements in cars: " << endl;
    for_each(cars.begin(), cars.end(), std::mem_fun_ref(&Car::display));
    cout << endl;

    for (size_t i = 0; i < pcars.size(); ++i)
        delete pcars[i];

    return 0;
}
```

- STL算法（有待于从C++primer上做理解性的补充）
>STL算法分类
>   不可变序列算法
>    可变序列算法
>    排序和搜索算法
>    数值算法
- <utility>头文件，将>=, <=, >都转化为对 < 的调用，！=则转换为==的调用，因此我们在操作符重载时可以只重载 < 和=两个操作符。同时要打开`using namespace std::rel_ops`

- 删除器代码片段：可以结合for_each删除区间内的所有指针
```
struct deleter{
template<class T>
void operator()(T* p){ delete p;}
};
for_each(container.begin(), container.end(), deleter());
```

- 唯一化数组元素并排序输出的代码段
```
    sort(nums.begin(), nums.end());
    auto end_unique = unique(nums.begin(), nums.end());
    nums.erase(end_unique, nums.end());
    copy(nums.begin(),nums.end(), ostream_iterator<int>(cout, "\n"));
```
- `substr(index, num)`表示从index开始截取num长的子串并返回

- count函数和count_if函数：功能类似于find。这个函数使用一对迭代器和一个值做参数，返回这个值出现次数的统计结果。
```
count(ivec.begin() , ivec.end() , searchValue)
bool greater10(int value)
 {
    return value >10;
 }
result1 = count_if(v1.begin(), v1.end(), greater10);
```
- 输出流
**cout输出的格式控制** ：使用操纵符，除了一次性的外，多数操纵符作用时间是直到下一次状态改变
```
#include <iomanip>
cout<<setw(10)<<nums[i]<<endl;//指定宽度，一次性的
cout << setiosflags(ios_base::left)<<nums[i]//左对齐
cout <<resetiosflags(ios_base::left)//取消左对齐的方式，恢复到默认右对齐，
cout<< setprecision(1) << values[i] << endl;//设置有效数字位数

cout<<setiosflags(ios_base::fixd)<< setprecision(1) << values[i] << endl;//设置有效数字
```
**将流写入二进制文件中**：当待存文件无需供人阅读时可以选用二进制的这种方式，读入读出效率都非常高
```
#include <fstream>
using namespace std;
struct Date { 
    int mon, day, year;  
};
int main() {
    Date dt = { 6, 10, 92 };
    ofstream file("date.dat", ios_base::binary);
    file.write(reinterpret_cast<char *>(&dt),sizeof(dt));
    file.close();
    return 0;
}
```
**字符串输出流**：典型应用是将数值转换为字符串，对于自定义类型，则必须重载相应的操作符如<<来使用
```
//11_6.cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

//函数模板toString可以将各种支持“<<“插入符的类型的对象转换为字符串。

template <class T>
inline string toString(const T &v) {
    ostringstream os;   //创建字符串输出流
    os << v;        //将变量v的值写入字符串流
    return os.str();    //返回输出流生成的字符串
}

int main() {
    string str1 = toString(5);
    cout << str1 << endl;
    string str2 = toString(1.2);
    cout << str2 << endl;
    return 0;
}

```

- 输入流: get 读可以带空字符，cin读不出空字符，getline则可以读出带空格的字符串。
```
例11-7 get函数应用举例
//11_7.cpp
#include <iostream>
using namespace std;
int main() {
    char ch;
    while ((ch = cin.get()) != EOF)//注意这里的EOF在unix下应该是crtl+Z那种东西
        cout.put(ch);
    return 0;
}
例11-8为输入流指定一个终止字符：
//11_8.cpp
#include <iostream>
#include <string>
using namespace std;
int main() {
    string line;
    cout << "Type a line terminated by 't' " << endl; 
    getline(cin, line, 't');
    cout << line << endl;
    return 0;
}
例11-9 从文件读一个二进制记录到一个结构中
//11_9.cpp
#include <iostream>
#include <fstream>
#include <cstring>
using namespace std;

struct SalaryInfo {
    unsigned id;
    double salary;
}; 
int main() {
    SalaryInfo employee1 = { 600001, 8000 };
    ofstream os("payroll", ios_base::out | ios_base::binary);
    os.write(reinterpret_cast<char *>(&employee1), sizeof(employee1));
    os.close();
    ifstream is("payroll", ios_base::in | ios_base::binary);
    if (is) {
        SalaryInfo employee2;
        is.read(reinterpret_cast<char *>(&employee2), sizeof(employee2));
        cout << employee2.id << " " << employee2.salary << endl;
    } else {
        cout << "ERROR: Cannot open file 'payroll'." << endl;
    }
    is.close();
    return 0;
}
例11-10用seekg函数设置位置指针
//11_10.cpp, 头部分省略
int main() {
    int values[] = { 3, 7, 0, 5, 4 };
    ofstream os("integers", ios_base::out | ios_base::binary);
    os.write(reinterpret_cast<char *>(values), sizeof(values));
    os.close();

    ifstream is("integers", ios_base::in | ios_base::binary);
    if (is) {
        is.seekg(3 * sizeof(int));
        int v;
        is.read(reinterpret_cast<char *>(&v), sizeof(int));
        cout << "The 4th integer in the file 'integers' is " << v << endl;
    } else {
        cout << "ERROR: Cannot open file 'integers'." << endl;
    }
    return 0;
}
例11-11 读一个文件并显示出其中0元素的位置
//11_11.cpp, 头部分省略
int main() {
    ifstream file("integers", ios_base::in | ios_base::binary);
    if (file) {
        while (file) {//读到文件尾file为0
            streampos here = file.tellg();
            int v;
            file.read(reinterpret_cast<char *>(&v), sizeof(int));
            if (file && v == 0) 
            cout << "Position " << here << " is 0" << endl;
        }
    } else {
        cout << "ERROR: Cannot open file 'integers'." << endl;
    }
    file.close();
    return 0;
}

```
**istringstream**的使用
```
template <class T>
inline T fromString(const string &str) {
    istringstream is(str);  //创建字符串输入流
    T v;
    is >> v;    //从字符串输入流中读取变量v
    return v;   //返回变量v
}

int main() {
    int v1 = fromString<int>("5");
    cout << v1 << endl;
    double v2 = fromString<double>("1.2");
    cout << v2 << endl;
    return 0;
}
输出结果：
5
1.2

```
- setprecision(2)处理浮点数时会自动的进行四舍五入，如12.3456在setprecision(2)后是12.35，想要得到12.34怎么办?用floor函数。
```
d=floor(d*100)/100;//处理后d=12.34
```
- 异常处理：首先要搞清楚为什么要引入异常处理，首先是为了让错误处理更加灵活，你这个模块没有资格处理错误时怎么办。其次是为了模块化的设计，集中处理异常，不打扰程序逻辑。在大型应用程序中异常处理机制更能凸显优势，需要不断应用学习。
```c++
//12_3.cpp
#include <iostream>
#include <cmath>
#include <stdexcept>
using namespace std;
//给出三角形三边长，计算三角形面积
double area(double a, double b, double c)  throw (invalid_argument)
{
   //判断三角形边长是否为正
    if (a <= 0 || b <= 0 || c <= 0)
        throw invalid_argument("the side length should be positive");
   //判断三边长是否满足三角不等式
    if (a + b <= c || b + c <= a || c + a <= b)
        throw invalid_argument("the side length should fit the triangle inequation");
   //由Heron公式计算三角形面积
    double s = (a + b + c) / 2; 
    return sqrt(s * (s - a) * (s - b) * (s - c));
}
int main() {
    double a, b, c; //三角形三边长
    cout << "Please input the side lengths of a triangle: ";
    cin >> a >> b >> c;
    try {
        double s = area(a, b, c);   //尝试计算三角形面积
        cout << "Area: " << s << endl;
    } catch (exception &e) {
        cout << "Error: " << e.what() << endl;
    }
    return 0;
}
```
- pq的使用，priority_queue 头文件<queue>,
```c++
        auto cmp = [](ListNode* p1, ListNode* p2){ return p1->val > p2->val;};
        priority_queue<ListNode*, vector<ListNode*>, decltype(cmp)> pq(cmp);
```