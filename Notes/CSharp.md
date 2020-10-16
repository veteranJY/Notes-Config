# C# Notes

[toc]

## Delegate & Event
```python
def a(a_str: str):
    print(a_str)

def b(a):
    a("output in b")

b(a)
```
Python里可以将一个函数作为参数传入另一个函数，然后C#里并不能这么直接。
- 为什么需要将函数作为参数传入？：
1. 可以构建func_map，解耦代码
2. 假设在C里调用B，B内部调用A，A需要一个参数来控制其内部流程
    ```CSharp
        public void static C()
        {
            ...
            flag = xxx.
            B(flag)
            ...
        }

        public void static B(flag)
        {
            ...
            A(flag)
            ...
        }
    ```
    B接收了一个与其无关的参数，如果能够传入函数的话，就可以写成
    ```CSharp
        public void static C()
        {
            ...
            flag = xxx.
            B(A(flag))
            ...
        }

        public void static B()
        {
            ...
            A(flag)
            ...
        }
    ```
    同样是解耦
3. 将委托delegate与事件绑定，可以实现更为复杂的功能。
在学习这部分的时候想起了pub/sub，当发生某些事件时，pub会将事件以及相关数据发送到对应的channel里，不同的channel对应不同的消费者，也即是sub。sub在接收消息前需要先注册一个channel，表明发到这个channel就会被这个sub对象接收。这也是发布/订阅模式的实现。

## Delegate & Event的使用
> Delegate可以被一个函数赋值，且可以代替函数接收参数，返回结果。

使用时先声明delegate，之后声明delegate对应的Event以及对应的EventArgs，一个Event可以注册多个函数，即：某个event发生时，该event可以被多个函数处理。且C#提供了很简便的注册方式，其内部重载了+=， -=运算符，使得用户通过+=，-=就可以为某个event增加或者删减注册函数。

以烧水壶烧水为例，在温度快到达100度时，烧水壶需要发出警报，同时显示屏上要显示出来温度。
- Heater类内部声明了deltegate，BoiledEventHandler，之后声明了delegate对应的event以及EventArgs
- BoilWater函数在温度大于95时会触发BoiledEvent，onBoiled函数内部回调所有绑定了该event的函数，即绑定时传进来的函数指针
- 在程序主逻辑中可以看到：先进行了event和函数的绑定，将event和display，alarm函数进行绑定，之后开始进行“烧水”，如果没有绑定event和函数，直接“烧水”，是不会触发报警的

> 回调: 把函数的指针（地址）作为参数传递给另一个函数，当这个指针被用来调用其所指向的函数时,就是回调函数。回调函数是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应。

delegate对象接收了函数指针，然后在事件被触发时通过指针调用函数，就是回调


```CSharp
using System;

namespace ConsoleApplication1 {
       // 热水器
       public class Heater {
              private int _temperature;
              public string Type = "RealFire 001";              // 添加型号作为演示
              public string Area = "China Xian";                     // 添加产地作为演示
              //声明委托
              public delegate void BoiledEventHandler(Object sender, BoiledEventArgs e);
              public event BoiledEventHandler Boiled;       //声明事件

              // 定义BoliedEventArgs类，传递给Observer所感兴趣的信息
              public class BoiledEventArgs : EventArgs {
                     public readonly int Temperature;
                     public BoiledEventArgs(int temperature) {
                            this.Temperature = temperature;
                     }
              }

              // 可以供继承自 Heater 的类重写，以便继承类拒绝其他对象对它的监视
              protected virtual void OnBolied(BoiledEventArgs e) {
                     if (Boiled != null) {       // 如果有对象注册
                            Boiled(this, e);       // 调用所有注册对象的方法
                     }
              }
             
              // 烧水
              public void BoilWater() {
                     for (int i = 0; i <= 100; i++) {
                            _temperature = i;
                            if (_temperature > 95) {
                                   //建立BoliedEventArgs 对象。
                                   BoiledEventArgs e = new BoiledEventArgs(_temperature);
                                   OnBolied(e);       // 调用 OnBolied方法
                            }
                     }
              }
       }

       // 警报器
       public class Alarm {
              public void MakeAlert(Object sender, Heater.BoiledEventArgs e) {
                     Heater heater = (Heater)sender;
                     //访问 sender 中的公共字段
                     Console.WriteLine("Alarm：{0} - {1}: ", heater.Area, heater.Type);
                     Console.WriteLine("Alarm: 嘀嘀嘀，水已经 {0} 度了：", e.Temperature);
                     Console.WriteLine();
              }

       }
       // 显示器
       public class Display {
              public static void ShowMsg(Object sender, Heater.BoiledEventArgs e) {       //静态方法
                     Heater heater = (Heater)sender;
                     Console.WriteLine("Display：{0} - {1}: ", heater.Area, heater.Type);
                     Console.WriteLine("Display：水快烧开了，当前温度：{0}度。", e.Temperature);
                     Console.WriteLine();
              }
       }

       class Program {
              static void Main() {
                     Heater heater = new Heater();
                     Alarm alarm = new Alarm();
                     // 注册语法
                     heater.Boiled += alarm.MakeAlert;
                     heater.Boiled += (new Alarm()).MakeAlert;              
                     heater.Boiled += new Heater.BoiledEventHandler(alarm.MakeAlert);       
                     heater.Boiled += Display.ShowMsg;   //注册静态方法

                     heater.BoilWater();       //烧水，会自动调用注册过对象的方法
              }
       }
}
```
