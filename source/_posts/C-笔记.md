---
title: 'C#笔记'
date: 2020-03-05 20:22:54
tags:
---

*懒癌患者缓慢更新进度（随便写一些东西）*
# 命名规范
1.规范

| 标志符 | 规则 | 实例与描述 |
| --- | --- | --- |
|类Class|Pascal|Application|
|枚举类型enum|Pascal|记住，是以Pascal命名，切勿包含Enum，否则FXCop会抛出Issue|
|委托delegate|Pascal|以Pascal命名，不以任何特殊字符串区别于类名、函数名|
|常量const|全部大写|全部大写，单词间以下划线隔开|
|接口interface|Pascal|IDisposable 注：总是以 I 前缀开始，后接Pascal命名|
|方法function|Pascal|ToString|
|命名空间namespace|Pascal|以.分隔，当每一个限定词均为Pascal命名方式，比如:using ExcelQuicker.Framework|
|参数|Camel|首字母小写|
|局部变量|Camel|也可以加入类型标识符，比如对于System.String类型，声明变量是以str开头，string strSQL = string.Empty;|
|数据成员|Camel|以m开头＋Pascal命名规则，如mProductType（m意味member）|
|属性|Pascal||

~~不一定要参照这个表，写在这里只是提醒自己最基本的东西~~

# 方法和作用域
## 声明方法
```
returnType methodName(parameterList)
{
    //这里添加代码主体
}
```

## 从方法返回数据
```
int addValues(int a,int b)
{
    //...
    return a + b;
}
```
如果不需要返回数据，则返回类型为void
```
void show(int answer)
{
    //...
    return;
}
```

## 使用表达式主体方法
C#允许以一种简化的方式写由单个表达式构成的方法。
```
int addValues(int a,int b) => a + b;

void show(int answer) => Console.WriteLine($"The answer is {answer}");
```
### 关于$(string interpolation 字符串插值)
当一个内插字符串被解析为一个结果字符串时，其将被代替
```
string name = "Mark";
var date = DateTime.Now;

//composite formatting:
Console.WriteLine("Hello,{0}! Today is {1}, it's {2:HH:mm} now.",name,date.DayOfWeek,date);
//string interpolation:
Console.WriteLine($"Hello,{name}! Today is {date.DayOfWeek}, it's {date:HH:mm} now.")
```
关于更多：https://docs.microsoft.com/zh-cn/dotnet/csharp/language-reference/tokens/interpolated

## 方法访问
调用对象上的方法就像访问字段
```
class Test{
    public double GetPi(){
        return 3.14;
    }
    static void Main(){
        Test test = new Test();
        double pi = test.GetPi();
        Console.WriteLine(pi);
    }
}
```

# 静态类和静态类成员
## 静态类
静态类基本上与非静态类相同，但存在一个差异：静态类无法实例化。 换句话说，无法使用`new`运算符创建类类型的变量。
由于不存在任何实例变量，因此可以使用类名本身访问静态类的成员。
静态类可以用作只对输入参数进行操作并且不必获取或设置任何内部实例字段的方法集的方便容器。 例如，在`.NET Framework`类库中，静态`System.Math`类包含执行数学运算，而无需存储或检索对`Math`类特定实例唯一的数据的方法。 即，通过指定类名和方法名称来应用类的成员，如下面的示例所示。
```
double dub = -3.14;  
Console.WriteLine(Math.Abs(dub));  
Console.WriteLine(Math.Floor(dub));  
Console.WriteLine(Math.Round(Math.Abs(dub)));  
  
// Output:  
// 3.14  
// -4  
// 3  
```
## 静态成员
非静态类可以包含静态方法、字段、属性或事件。 即使未创建类的任何实例，也可对类调用静态成员。 静态成员始终按类名（而不是实例名称）进行访问。 静态成员只有一个副本存在（与创建的类的实例数有关）。 静态方法和属性无法在其包含类型中访问非静态字段和事件，它们无法访问任何对象的实例变量，除非在方法参数中显式传递它。
更典型的做法是声明具有一些静态成员的非静态类（而不是将整个类都声明为静态）。 静态字段的两个常见用途是保留已实例化的对象数的计数，或是存储必须在所有实例间共享的值。
静态方法可以进行重载，但不能进行替代，因为它们属于类，而不属于类的任何实例。

# 管理错误和异常

# 流与I/O （等待
System.IO命名空间
1.Stream类
-按存取位置分类[FileStream,MemoryStream,BufferedStream]
2.读写类
-BinaryReader和BinaryWriter
-TextReader和TextWriter[StreamReader,StreamWriter,StringReader,StringWriter]
关系：
```
FileStream fs = new FileStream(@"c:\temp\foo.txt",FileMode.Create);
StreamWriter writer = new StreamWriter(fs);
```


# 数组

# 委托和事件
## 委托
委托可以提高代码的可扩展性和灵活性。
委托是安全封装方法的类型，类似于 C 和 C++ 中的函数指针。 与 C 函数指针不同的是，委托是面向对象的、类型安全的和可靠的。 委托的类型由委托的名称确定。
以下示例声明名为 Del 的委托，该委托可以封装采用字符串作为参数并返回 void 的方法：
```
public delegate void Del(string message);

// Create a method for a delegate.
public static void DelegateMethod(string message)
{
    Console.WriteLine(message);
}

// Instantiate the delegate.
Del handler = DelegateMethod;

// Call the delegate.
handler("Hello World");
```
### 使用操作符
使用+=操作符让实例引用匹配的方法，使用-=移除方法。

为了使委托的主体独立于各种方法的主体，有几种选项：
1.将委托变量声明为公共的 
```
public MyDelegate pDelegate;
```
2.保持私有，提供读写属性来访问
```
private MyDelegate pDelegate;

public MyDelegate PDelegate{
    get{
        return this.pDelegate;
    }
    set{
        this.pDelegate = value;
    }
}
```
3.实现单独的添加/删除方法来提供完全的封装性
```
public void Add(MyDelegate myMethod){
    this.pDelegate += myMethod;
}
```
## 事件
类 或对象可以通过事件向其他类或对象通知发生的相关事情。 发送（或引发 ）事件的类称为“发布者” ，接收（或处理 ）事件的类称为“订阅者” 。
### 声明事件
用以下语法声明事件：
```
event delegateTypeName eventName //delegateTypeName是委托类型名称，eventName是事件名称。
```
### 订阅事件
类似于委托，事件也用+=操作符进行订阅。
```
class TemperatureMonitor
{
    public delegate void StopMachineDelegate();
    public event StopMachineDelegate MachineOverHeating;
}

TemperatureMonitor.MachineOverHeating += painter.PaintOff;
//当MachineOverHeating事件发生时，会调用painter.PaintOff方法。
```
对应地，-=操作符用于取消订阅事件。
### 引发事件
事件可以像方法一样调用来引发。引发事件后，所有和事件关联的委托会被依次调用。
```
class TemperatureMonitor
{
    public delegate void StopMachineDelegate;
    public event StopMachineDelegate MachineOverHeating;

    private void Notify()
    {
        if(this.MachineOverHeating != null)
        {
            this.MachineOverHeating();
        }
    }
}
```




