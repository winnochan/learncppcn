---
title: 17.2 - C++继承基础
alias: 17.2 - C++继承基础
origin: /basic-inheritance-in-c/
origin_title: "17.2 — Basic inheritance in C++"
time: 2020-12-21
type: translation
tags:
- inheritance
---

??? note "关键点速记"
	
	-

从抽象层面讨论过继承之后，让我们来看看 C++ 中的继承吧。

C++的继承发生在类与类之间。在继承关系中，被继承的类称为[[parent-class|父类(parent class)]]、[[base-class|基类(base class)]]或[[super-class|超类(super class)]]。而继承父类的类，则称为[[child-class|子类(child class / sub class)]]、[[derived-class|派生类]]。

![](http://learncpp.com/images/CppTutorial/Section11/FruitInheritance.gif)

在上图中，水果是父类，苹果和香蕉都是子类。

![](http://learncpp.com/images/CppTutorial/Section11/ShapesInheritance.gif)

在上图中，三角形既是子类（*形状*的子类），也是父类（对直角三角形而言）。

子类从父类继承行为(成员函数)和属性(成员变量)(需要遵守访问限制，我们将在以后的课程中讨论)。
这些变量和函数成为派生类的成员。

因为子类也是一个标准的类，所以它们(当然)可以有自己的类成员。


## `Person` 类**

下面的例子中使用 `Person` 表示一个”人类“：

```cpp
#include <string>

class Person
{
// In this example, we're making our members public for simplicity
public:
    std::string m_name{};
    int m_age{};

    Person(const std::string& name = "", int age = 0)
        : m_name{ name }, m_age{ age }
    {
    }

    const std::string& getName() const { return m_name; }
    int getAge() const { return m_age; }

};
```

因为这个`Person`类被设计用来表示一般的人，所以我们只定义了任何类型的人都通用的成员。每个人(不论性别、职业等)都有名字和年龄，所以在这里表示出来。

注意，在本例中，我们将所有变量和函数设置为公共的。这纯粹是为了使这些示例保持简单。通常我们会将变量设为私有。我们将在本章后面讨论访问控制以及它们如何与继承交互。


## `BaseballPlayer` 类

Let’s say we wanted to write a program that keeps track of information about some baseball players. Baseball players need to contain information that is specific to baseball players -- for example, we might want to store a player’s batting average, and the number of home runs they’ve hit.

假设我们想要编写一个程序来记录棒球运动员的信息。棒球运动员需要包含特定的关于棒球运动员的信息——例如，球员的击球率和本垒打数。

下面是一个不完整的版本：

```cpp
class BaseballPlayer
{
// In this example, we're making our members public for simplicity
public:
    double m_battingAverage{};
    int m_homeRuns{};

    BaseballPlayer(double battingAverage = 0.0, int homeRuns = 0)
       : m_battingAverage{battingAverage}, m_homeRuns{homeRuns}
    {
    }
};
```

我们同时还需要记录运动员的姓名和年龄，而 `Person`类中正好包含这些信息。

我们有三种方式可以为 `BaseballPlayer` 类添加姓名和年龄：

1. 直接将姓名和年龄作为成员添加到`BaseballPlayer`类中。这可能是最糟糕的选择，因为我们复制的是`Person`类中已经存在的代码。对`Person`的任何更新都必须在`BaseballPlayer`中进行。
2. 使用组合将`Person`添加为`BaseballPlayer`的成员。但我们必须问自己，“一个棒球运动员有一个人吗？” 并不是。所以这不是正确的范式。
3. 让`BaseballPlayer`从`Person`继承这些属性。记住，继承表示`is-a`关系。棒球运动员是人吗？当然是。所以继承是一个很好的选择。


## 让 `BaseballPlayer` 成为派生类

让 `BaseballPlayer` 继承 `Person` 类的语法很简单。在完成 `class BaseballPlayer` 声明后，使用一个冒号，外加 `public`关键字，接上需要继承的类的名字即可。这种方式称为[[public-inheritance|公开继承]]。我们会在后面的课程中再详细讨论公开继承的话题。

```cpp
// BaseballPlayer publicly inheriting Person
class BaseballPlayer : public Person
{
public:
    double m_battingAverage{};
    int m_homeRuns{};

    BaseballPlayer(double battingAverage = 0.0, int homeRuns = 0)
       : m_battingAverage{battingAverage}, m_homeRuns{homeRuns}
    {
    }
};
```


使用派生关系图，我们的继承看起来像这样：

![](https://www.learncpp.com/images/CppTutorial/Section11/BaseballPlayerInheritance.gif)

当 `BaseballPlayer` 继承 `Person`后，`BaseballPlayer` 就获取了 `Person` 的[[member-variable|成员变量]]和[[member-function|成员函数]]。此外，`BaseballPlayer` 还定义了两个自己特有的成员：`m_battingAverage` 和 `m_homeRuns`。这么做的原因很简单，因为这些属性是属于 `BaseballPlayer` 的，而不是任何 `Person` 所共有的。

因此，`BaseballPlayer` 对象最终会包含4个成员变量：`m_battingAverage` 和 `m_homeRuns` 是 `BaseballPlayer` 特有的，`m_name` 和 `m_age` 是继承自 `Person` 的。

上面论述很容易证明：

```cpp
#include <iostream>
#include <string>

class Person
{
public:
    std::string m_name{};
    int m_age{};

    Person(const std::string& name = "", int age = 0)
        : m_name{name}, m_age{age}
    {
    }

    const std::string& getName() const { return m_name; }
    int getAge() const { return m_age; }

};

// BaseballPlayer publicly inheriting Person
class BaseballPlayer : public Person
{
public:
    double m_battingAverage{};
    int m_homeRuns{};

    BaseballPlayer(double battingAverage = 0.0, int homeRuns = 0)
       : m_battingAverage{battingAverage}, m_homeRuns{homeRuns}
    {
    }
};

int main()
{
    // Create a new BaseballPlayer object
    BaseballPlayer joe{};
    // Assign it a name (we can do this directly because m_name is public)
    joe.m_name = "Joe";
    // Print out the name
    std::cout << joe.getName() << '\n'; // use the getName() function we've acquired from the Person base class

    return 0;
}
```

程序输出：

```
Joe
```

代码可以正确地编译运行，因为`joe` 是 `BaseballPlayer`，而所有的 `BaseballPlayer` 对象都具有 `m_name` 成员变量和 `getName()` 成员函数，它们是从 `Person` 类继承过来的。

## `Employee` 派生类

Now let’s write another class that also inherits from Person. This time, we’ll write an Employee class. An employee “is a” person, so using inheritance is appropriate:

```cpp
// Employee publicly inherits from Person
class Employee: public Person
{
public:
    double m_hourlySalary{};
    long m_employeeID{};

    Employee(double hourlySalary = 0.0, long employeeID = 0)
        : m_hourlySalary{hourlySalary}, m_employeeID{employeeID}
    {
    }

    void printNameAndSalary() const
    {
        std::cout << m_name << ": " << m_hourlySalary << '\n';
    }
};
```

COPY

Employee inherits m_name and m_age from Person (as well as the two access functions), and adds two more member variables and a member function of its own. Note that printNameAndSalary() uses variables both from the class it belongs to (Employee::m_hourlySalary) and the parent class (Person::m_name).

This gives us a derivation chart that looks like this:

![](https://www.learncpp.com/images/CppTutorial/Section11/EmployeeInheritance.gif)

Note that Employee and BaseballPlayer don’t have any direct relationship, even though they both inherit from Person.

Here’s a full example using Employee:

```cpp
#include <iostream>
#include <string>

class Person
{
public:
    std::string m_name{};
    int m_age{};

    const std::string& getName() const { return m_name; }
    int getAge() const { return m_age; }

    Person(const std::string& name = "", int age = 0)
        : m_name{name}, m_age{age}
    {
    }
};

// Employee publicly inherits from Person
class Employee: public Person
{
public:
    double m_hourlySalary{};
    long m_employeeID{};

    Employee(double hourlySalary = 0.0, long employeeID = 0)
        : m_hourlySalary{hourlySalary}, m_employeeID{employeeID}
    {
    }

    void printNameAndSalary() const
    {
        std::cout << m_name << ": " << m_hourlySalary << '\n';
    }
};

int main()
{
    Employee frank{20.25, 12345};
    frank.m_name = "Frank"; // we can do this because m_name is public

    frank.printNameAndSalary();

    return 0;
}
```

COPY

This prints:

```
Frank: 20.25
```

## 继承链

It’s possible to inherit from a class that is itself derived from another class. There is nothing noteworthy or special when doing so -- everything proceeds as in the examples above.

For example, let’s write a Supervisor class. A Supervisor is an Employee, which is a Person. We’ve already written an Employee class, so let’s use that as the base class from which to derive Supervisor:

```cpp
class Supervisor: public Employee
{
public:
    // This Supervisor can oversee a max of 5 employees
    long m_overseesIDs[5]{};
};
```

COPY

Now our derivation chart looks like this:

![](https://www.learncpp.com/images/CppTutorial/Section11/SupervisorInheritance.gif)

All Supervisor objects inherit the functions and variables from both Employee and Person, and add their own m_overseesIDs member variable.

By constructing such inheritance chains, we can create a set of reusable classes that are very general (at the top) and become progressively more specific at each level of inheritance.

## 为什么此类继承是有用的？

Inheriting from a base class means we don’t have to redefine the information from the base class in our derived classes. We automatically receive the member functions and member variables of the base class through inheritance, and then simply add the additional functions or member variables we want. This not only saves work, but also means that if we ever update or modify the base class (e.g. add new functions, or fix a bug), all of our derived classes will automatically inherit the changes!

For example, if we ever added a new function to Person, both Employee and Supervisor would automatically gain access to it. If we added a new variable to Employee, Supervisor would also gain access to it. This allows us to construct new classes in an easy, intuitive, and low-maintenance way!

## 结论

Inheritance allows us to reuse classes by having other classes inherit their members. In future lessons, we’ll continue to explore how this works.