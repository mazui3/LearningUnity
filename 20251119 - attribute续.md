看班码对以下几种做了自定义attribute。
>说起来这些代码可能是个库来的！

```cs
[AttributeUsage(AttributeTargets.Class)]
public class ChildOfAttribute : Attribute
{
    public Type type;

    public ChildOfAttribute(Type type = null)
    {
        this.type = type;
    }
}
```

感到疑惑，跟friendOf长得非常像。\
Q:Type 是一个类，位于 System 命名空间中。它代表了类型本身的元数据。

```cs
// Type 对象包含了关于某个类型的所有信息
Type stringType = typeof(string);    // 获取string类型的Type对象
Type intType = typeof(int);          // 获取int类型的Type对象

// Type对象可以告诉你关于类型的信息：
Console.WriteLine(stringType.Name);      // 输出: "String"
Console.WriteLine(stringType.FullName);  // 输出: "System.String"
Console.WriteLine(stringType.IsClass);   // 输出: "True"
```

这么一想，String算不算也是一种class，用string做出来的比如String Name = “someBody”的someBody就是一个String object…？

```cs
// 使用 typeof 获取类型的元数据
[ChildOf(typeof(Animal))]  // typeof(Animal) 返回 Animal 类的 Type 对象
public class Dog { }

Animal myAnimal = new Dog();

// typeof - 编译时就知道类型
Type animalType = typeof(Animal); // 总是返回Animal的Type

// GetType() - 运行时确定实际类型  
Type actualType = myAnimal.GetType(); // 返回Dog的Type（因为myAnimal实际是Dog）

[ChildOf(typeof(Animal))]  // 这里：typeof(Animal) 返回一个Type对象
public class Dog { }

// 对应的特性定义
public class ChildOfAttribute : Attribute
{
    public Type type;  // 这里存储的就是上面传来的 Type 对象
    
    public ChildOfAttribute(Type type = null)
    {
        this.type = type;  // 将Animal的Type对象存储起来
    }
}

// 获取Dog类上的ChildOf特性
var attribute = typeof(Dog).GetCustomAttribute<ChildOfAttribute>();

if (attribute?.type != null)
{
    Console.WriteLine($"Dog 的父类是: {attribute.type.Name}"); // 输出: "Animal"
    Console.WriteLine($"父类的完整名称: {attribute.type.FullName}"); // 输出: "YourNamespace.Animal"
    
    // 还可以获取更多信息
    Console.WriteLine($"是类吗? {attribute.type.IsClass}"); // 输出: "True"
    Console.WriteLine($"是接口吗? {attribute.type.IsInterface}"); // 输出: "False"
}

// 建立类型关系网
[ChildOf(typeof(Mammal))]
public class Dog { }

[ChildOf(typeof(Mammal))]
public class Cat { }

[ChildOf(typeof(Animal))]
public class Mammal { }

[ChildOf] // type = null，表示是根类型
public class Animal { }

// 查询工具
public static void PrintParent(Type childType)
{
    var attr = childType.GetCustomAttribute<ChildOfAttribute>();
    if (attr?.type != null)
    {
        Console.WriteLine($"{childType.Name} 的父类是 {attr.type.Name}");
    }
    else
    {
        Console.WriteLine($"{childType.Name} 是根类型");
    }
}
```

Q: FriendOfAttribute 和ChildOfAttribute 的区别在哪里
A: 虽然代码结构相似，但它们服务于完全不同的设计目的

它们的header挺重要的（metadata），也是为数不多的区别。

班码里有两个attribute差别比较大。
```cs
/// <summary>
/// 静态字段需加此标签
/// valueToAssign: 初始化时的字段值
/// assignNewTypeInstance: 从默认构造函数初始化
/// </summary>
[AttributeUsage(AttributeTargets.Field)]
public class StaticFieldAttribute: Attribute
{
    public readonly object valueToAssign;

    public readonly bool assignNewTypeInstance;
    
    public StaticFieldAttribute()
    {
        this.valueToAssign  = null;
        this.assignNewTypeInstance = false;
    }
    public StaticFieldAttribute(object valueToAssign )
    {
        this.valueToAssign  = valueToAssign ;
        this.assignNewTypeInstance = false;
    }
    
    public StaticFieldAttribute(bool assignNewTypeInstance)
    {
        this.valueToAssign  = null;
        this.assignNewTypeInstance = assignNewTypeInstance;
    }
}
 
```
