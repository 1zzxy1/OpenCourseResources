# 第七章作业

## 7-5

定义一个基类Shape，在此基础上派生出Rectangle和Circle，二者带有getArea()函数计算对象的面积。使用Rectangle类创建一个派生类Square()。

源码：

```cpp
#include <bits/stdc++.h>

class Shape
{
public:
    virtual double getArea() = 0;
};

class Rectangle : public Shape
{
protected:
    double length;
    double width;

public:
    Rectangle(double l, double w) : length(l), width(w) {};

    double getArea()
    {
        return 0.5 * length * width;
    };
};

class Circle : public Shape
{
private:
    double radius;

public:
    Circle(double r) : radius(r) {}
    double getArea()
    {
        return 3.14159 * radius * radius;
    }
};

class Square : public Rectangle
{
public:
    Square(double s) : Rectangle(s,s) {};
    double getArea()
    {
        return length * width;
    };
};

int main() {
    Rectangle rect(3, 4);
    Circle circle(5);
    Square square(2);

    std::cout << "Rectangle area: " << rect.getArea() << std::endl;
    std::cout << "Circle area: " << circle.getArea() << std::endl;
    std::cout << "Square area: " << square.getArea() << std::endl;

    return 0;
}
```

运行结果：

```Powershell
PS C:\Users\fQwQf\Desktop\project\WHU_ALP_2024\chapter7> g++ .\7-5.cpp
PS C:\Users\fQwQf\Desktop\project\WHU_ALP_2024\chapter7> ./a.exe
Rectangle area: 6
Circle area: 78.5397
Square area: 4
```

## 7-11

定义一个基类BaseClass，从它派生出DerivedClass，BaseClass有成员函数fn1()、fn2()，DerivedClass也有成员函数fn1()、fn2()，在主函数中声明一个DerivedClass的对象，分别用DerivedClass的对象以及BaseClass和DerivedClass的指针来调用fn1()、fn2()，观察运行结果。  

源码：

```cpp
#include <bits/stdc++.h>

class BaseClass{
public:
    void fn1(){
        std::cout << "BaseClass fn1" << std::endl;
    }

    void fn2(){
        std::cout << "BaseClass fn2" << std::endl;
    }
};

class DerivedClass : public BaseClass{
public:
    void fn1(){
        std::cout << "DerivedClass fn1" << std::endl;
    }

    void fn2(){
        std::cout << "DerivedClass fn2" << std::endl;
    }
};

int main(){
    DerivedClass d;
    BaseClass *b = &d;
    DerivedClass *c = &d;
    d.fn1();
    d.fn2();
    b->fn1();
    b->fn2();
    c->fn1();
    c->fn2();
}
```

运行结果：

```Powershell
PS C:\Users\fQwQf\Desktop\project\WHU_ALP_2024\chapter7> g++ .\7-11.cpp
PS C:\Users\fQwQf\Desktop\project\WHU_ALP_2024\chapter7> .\a.exe
DerivedClass fn1
DerivedClass fn2
BaseClass fn1
BaseClass fn2
DerivedClass fn1
DerivedClass fn2
```

分析：  

1. d.fn1() 和 d.fn2()

    d 是 DerivedClass 类型的对象，调用 fn1() 和 fn2() 时会根据 静态绑定 规则直接调用派生类 DerivedClass 中的函数，因为 fn1() 和 fn2() 在 DerivedClass 中都有定义并覆盖了基类的同名函数。所以，这两行会输出：

    ```Powershell
    DerivedClass fn1
    DerivedClass fn2
    ```

2. b->fn1() 和 b->fn2()

    b 是 BaseClass 类型的指针，指向 DerivedClass 对象。由于 fn1() 和 fn2() 不是虚函数（没有使用 virtual 关键字），所以它们是 静态绑定的。  
    也就是说，当通过 BaseClass 类型的指针 b 调用 fn1() 和 fn2() 时，编译器会在编译时决定调用基类中的 fn1() 和 fn2()，即使 b 实际指向的是 DerivedClass 对象。所以，这两行会输出：

    ```Powershell
    BaseClass fn1
    BaseClass fn2
    ```

3. c->fn1() 和 c->fn2()

    c 是 DerivedClass 类型的指针，指向 DerivedClass 对象。在这种情况下，编译器知道 c 指向的是 DerivedClass 类型的对象，直接调用 DerivedClass 中的 fn1() 和 fn2()。因此，这两行会输出：

    ```Powershell
    DerivedClass fn1
    DerivedClass fn2
    ```
