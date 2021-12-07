
<a id="design-pattern"></a>

## 📏 设计模式

> 各大设计模式例子参考：[CSDN专栏 . C++ 设计模式](https://blog.csdn.net/liang19890820/article/details/66974516) 系列博文

[设计模式工程目录](DesignPattern)

### 单例模式

[单例模式例子](DesignPattern/SingletonPattern)

[Meyers Singleton](http://laristra.github.io/flecsi/src/developer-guide/patterns/meyers_singleton.html)

```
struct singleton_t
{

  static
  singleton_t &
  instance()
  {
    static singleton_t s;
    return s;
  } // instance

  singleton_t(const singleton_t &) = delete;
  singleton_t & operator = (const singleton_t &) = delete;

private:

  singleton_t() : value_(0) {}
  ~singleton_t() {}
  int value_;

}; // struct singleton_t
```
注意，这里删除了`拷贝赋值`和`拷贝构造`，并且将`默认构造`和`默认析构`函数设置为private()；
取实例仍使用函数接口，而不是直接取内部变量，该函数是静态成员函数，返回类型是类的引用(不是指针了)；静态类对象在该函数内部，这样的话应该不调用时不实例化。


### 抽象工厂模式

[抽象工厂模式例子](DesignPattern/AbstractFactoryPattern)

### 适配器模式

[适配器模式例子](DesignPattern/AdapterPattern)

### 桥接模式

[桥接模式例子](DesignPattern/BridgePattern)

### 观察者模式

[观察者模式例子](DesignPattern/ObserverPattern)

### 设计模式的六大原则

* 单一职责原则（SRP，Single Responsibility Principle）
* 里氏替换原则（LSP，Liskov Substitution Principle）
* 依赖倒置原则（DIP，Dependence Inversion Principle）
* 接口隔离原则（ISP，Interface Segregation Principle）
* 迪米特法则（LoD，Law of Demeter）
* 开放封闭原则（OCP，Open Close Principle）
