## ⭕ C++的多态
C++ 的多态体现在「重载」和「重写」。

### 编译时多态
一个函数存在不同的参数列表，即函数的「重载」。

### 运行时多态 -- 虚函数
虚函数是C++标准所制定的，实现虚函数时，各大编译器都采用「虚函数表」。

「虚函数表」是保存类内虚函数地址的表格，一个类，共享同一个虚函数表。
「虚函数表指针」是指向虚函表的指针，其由类的实例持有。

#### 构造函数和析构函数可以是虚函数吗？

> ref: Effective C++, Item7 

构造函数不能是虚函数，因为在构造函数执行前，类的虚函数表指针是空的。
析构函数最好是虚函数，因为一个父类指针可能指向子类对象，在这种情况下，只有构造函数为虚函数，才能保证对子类进行析构，避免内存泄漏。

当存在一个函数为虚函数时，需要将析构函数生命为虚函数以防止潜在的内存泄露。
但不存在虚函数时，则不需要将析构函数生命为虚函数，可以节省虚函数指针与虚函数表的额外开销。


## ⭕ 静态链接(库)和动态链接(库)

> ref: https://zhuanlan.zhihu.com/p/83716863

在一个程序的编译过程中，分为以下几个步骤：预处理，编译，汇编，链接。其中链接的库分为两种「静态链接库:」和「动态链接库」，前者对应`.a`/`.lib`， 后者对应`.so`/`.dll`。

对于「静态链接库」而言在链接阶段，会将汇编生成的「目标文件.o」与引用到的库一起链接打包到可执行文件中。因此对应的链接方式称为静态链接。
- 静态链接库对函数库的链接是放在编译时期完成的。程序在运行时与函数库就没有了任何的联系。
- 它可能比较浪费空间和资源，因为所有相关的目标文件与牵涉到的函数库被链接合成一个可执行文件。
- 静态库对程序的更新和发布也会带来麻烦。如果静态库更新了，所有使用它的应用程序都需要重新编译、部署、发布给用户。

「动态链接库」在程序编译时并不会被连接到目标代码中，而是在程序运行时才被载入。
- 不同的应用程序如果调用相同的库，那么在内存里只需要有一份该共享库的实例，可以实现进程之间的资源共享。（因此动态库也称为共享库）规避了空间浪费问题。
- 动态库在程序运行时才被载入，也解决了静态库对程序的更新、部署和发布带来的麻烦。用户只需要更新动态库即可将一些程序升级变得简单，增量更新。

## ⭕ Static关键字

- 静态成员变量（面向对象）
- 静态成员函数（面向对象）
- 静态全局变量（面向过程）
- 静态局部变量（面向过程）
- 静态函数（面向过程）

## ⭕ 类型转换

### static_cast
  
任何编写程序时能够明确的类型转换都可以使用static_cast（static_cast不能转换掉底层const，volatile和__unaligned属性）。由于不提供运行时的检查，所以叫static_cast，因此，需要在编写程序时确认转换的安全性。主要在以下几种场合中使用：用于类层次结构中，父类和子类之间指针和引用的转换；进行上行转换，把子类对象的指针/引用转换为父类指针/引用，这种转换是安全的；进行下行转换，把父类对象的指针/引用转换成子类指针/引用，这种转换是不安全的，需要编写程序时来确认；用于基本数据类型之间的转换，例如把int转char，int转enum等，需要编写程序时来确认安全性；把void指针转换成目标类型的指针（这是极其不安全的）

### dynamic_cast
相比static_cast，dynamic_cast会在运行时检查类型转换是否合法，具有一定的安全性。由于运行时的检查，所以会额外消耗一些性能。dynamic_cast使用场景与static相似，在类层次结构中使用时，上行转换和static_cast没有区别，都是安全的；下行转换时，dynamic_cast会检查转换的类型，相比static_cast更安全。


### const_cast

常量指针被转换成非常量指针，并且仍然指向原来的对象；常量引用被转换成非常量引用，并且仍然引用原来的对象。

### reinterpret_cast
非常激进的指针类型转换，在编译期完成，可以转换任何类型的指针，所以极不安全。非极端情况不要使用。

