[Microsoft](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/reflection-and-attributes/)\
[Geeks](https://www.geeksforgeeks.org/c-sharp/attributes-in-c-sharp/)
[StackOverflow](https://stackoverflow.com/questions/3197265/c-sharp-attributes-and-their-uses)

上回在metadata的那章里说到attribute就是带方框的，写在代码之上当作标签/限制的东西。\
attribute的param分为positional和named。
```cs
[DllImport("user32.dll")]
[DllImport("user32.dll", SetLastError=false, ExactSpelling=false)]
[DllImport("user32.dll", ExactSpelling=false, SetLastError=false)]
```
这里DLL name, is positional and always comes first.\
The other instances are named parameters.

Positional parameters\
Parameters of the attribute constructor:

- Must specify, can't omit
- Always specify first
- Specify in certain order

Named parameters\
Properties or fields of the attribute:

- Always optional, omit when false
- Specify after positional parameters
- Specify in any order

An attribute can apply to a class, a method, or an assembly. By default, an attribute applies to the element that follows it.\
But you can also explicitly identify the element to associate, such as a method, a parameter, or the return value.

```cs
// default: applies to method
[ValidatedContract]
int Method1() { return 0; }

// applies to method
[method: ValidatedContract]
int Method2() { return 0; }

// applies to parameter
int Method3([ValidatedContract] string contract) { return 0; }

// applies to return value
[return: ValidatedContract]
int Method4() { return 0; }
```
具体想要限定什么地方用什么keyword得翻一下microsoft的表格。 

\
[Define and read custom attributes](https://learn.microsoft.com/en-us/dotnet/csharp/advanced-topics/reflection-and-attributes/attribute-tutorial)

回到班码。
```cs
[AttributeUsage(AttributeTargets.Class, AllowMultiple = true, Inherited = false)]
public class FriendOfAttribute : Attribute
{
    public Type Type;
    
    public FriendOfAttribute(Type type)
    {
        this.Type = type;
    }
}
```
A: 这段代码定义了一个自定义特性（Attribute）叫做 FriendOfAttribute，它的作用是：\
- 声明特性目标：[AttributeUsage] 指定这个特性只能用于类（AttributeTargets.Class）
- 允许多次使用：AllowMultiple = true 表示同一个类上可以多次使用这个特性
- 不继承：Inherited = false 表示子类不会继承这个特性

这个特性用于建立"友元关系"，标记某个类可以访问另一个类的内部成员（类似于C++中的friend概念）。

```cs
[FriendOf(typeof(ClassA))]
[FriendOf(typeof(ClassB))]  // 允许多个FriendOf特性
public class MyClass
{
    // 这个类被标记为ClassA和ClassB的友元
}

public class ClassA
{
    // MyClass可以访问ClassA的内部成员
    private int privateField;
    
    // 通过反射检查友元关系，决定是否允许访问
}
```
