# DesignPattern Notes

[toc]

## 创建型模式
### 简单工厂模式
对象是工厂里的产品，使用者不需要关心具体对象如何创建，只需要传入一个正确的参数，就可以获得需要的对象
应用如获取不同编码方式的编码器，encoder = Encoder('utf-8')

### 工厂方法模式
进一步，将工厂也抽象，工厂父类不负责具体产品对象的创建，一个工厂子类去负责一个具体对象的创建，使用时只需要创建对应工厂，创建对象被延迟。同时，简单工厂中的判断逻辑被拿到外面，而不是在工厂里。

### 抽象工厂模式
创建产品的业务进一步复杂：
1. 产品之间出现层级结构关系
2. 两个产品虽然同属于同一抽象产品，但是他们需要不同的工厂来生产，即出现产品-工厂绑定关系

抽象工厂模式是工厂方法的进一步抽象，具体工厂内部产生先产生抽象产品，后产生具体产品。不同工厂产生相同的抽象产品，但是产生的具体产品由具体工厂内部决定。

### 建造者模式
该模式的目标是分离一个复杂对象的创建和表示，即将对象创建的所需的复杂步骤（复杂的逻辑，需要读表，网络请求等）抽离出来，在外部创建对象时可以一步一步创建，每一步可以获得不同的对象。

### 单例模式
有些情况下，全局只需要一个对象（如计数器，数据库conn对象）。一般是用一个对象A去持有一个私有的类对象，该对象不会被外部new，每次获取该对象必须要通过A对象去获取，就可以保证单例。
1. 最简单的实现方式 ------ lazy loading
```C#
private static readonly Singleton instance = null;
public static Singleton getInstance()
{
    if (instance != singleton)
    {
        singlelton = new Singleton();
    }
}
```
这种方式在多线程并发的情况下，极容易产生：多个线程同时判断instance为空，然后多个线程同时创建对象的情况。因此该方式并不是线程安全的。

2. 为了保证线程安全，使用lock关键字
```C#
public class Singleton<T> where T : class, new()
{
    private static readonly T _instance;
    private static readonly object syslock = new object();
 
    public static T Instance
    {
        get
        {
            if (_instance == null)
            {
                lock (syslock)
                {
                    if (_instance == null)
                    {
                        _instance = new T();
                    }
                }
            }
            return _instance;
        }
    }
} 
```
## 行为型模式
### 观察者模式 （发布/订阅）
