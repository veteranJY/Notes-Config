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
Python里可以将一个函数作为参数传入另一个函数，然后C#里并不能这么做。
- 为什么需要将函数作为参数传入？：
第一：可以构建func_map，解耦代码
第二：假设在C里调用B，B内部调用A，A需要一个参数来控制其内部流程
    ```C#
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
    ```C#
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
第三：将委托delegate与事件绑定，可以实现更为复杂的功能。
在学习这部分的时候想起了pub/sub，当发生某些事件时，pub会将事件以及相关数据发送到对应的channel里，不同的channel对应不同的消费者，也即是sub。sub在接收消息前需要先注册一个channel，表明发到这个channel就会被这个sub对象接收。这也是发布/订阅模式的实现。
    ```C#
    //声明委托
    public delegate void BoiledEventHandler(Object sender, BoliedEventArgs e);
    public event BoiledEventHandler Boiled;       //声明事件

    // 定义BoliedEventArgs类，传递给Observer所感兴趣的信息
    public class BoliedEventArgs : EventArgs {
            public readonly int temperature;
            public BoliedEventArgs(int temperature) {
                this.temperature = temperature;
            }
    }
    ```

## Delegate & Event的使用
Delegate可以被一个函数赋值，且可以代替函数接收参数，返回结果。
使用时先声明delegate，之后声明delegate对应的Event以及对应的EventArgs，一个Event可以注册多个函数，即：某个event发生时，该event可以被多个函数处理。且C#提供了很简便的注册方式，通过+=，-=就可以为某个event增加或者删减注册函数
