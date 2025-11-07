因为回调写得一塌糊度，查了查，发现这俩不是一块出现的啊。

https://learn.microsoft.com/en-us/dotnet/csharp/distinguish-delegates-events
>The most important consideration in determining which language feature to use is whether or not there must be an attached subscriber.
>If your code must call the code supplied by the subscriber, you should use a design based on delegates when you need to implement callback.
>If your code can complete all its work without calling any subscribers, you should use a design based on events.

https://stackoverflow.com/questions/29155/what-are-the-differences-between-delegates-and-events
>An Event declaration adds a layer of abstraction and protection on the delegate instance.
>This protection prevents clients of the delegate from resetting the delegate and its invocation list and only allows adding or removing targets from the invocation list.

等一下这两个在说不同的事情.jpg

用法上event还是比较直白的.我把它理解为function placeholder.\
在某个情况下要跑的functions们就一起连上,最后在那个情况下一起invoke.\
比如onPageExit += dealWithDataSaving; 这样?

delegate需要param.\
我感觉delegate有点像function blueprint.(因为式子structure是先define好的?)
>https://www.geeksforgeeks.org/c-sharp/delegates-c-sharp/

```cs
public class DelegateExample
{
    // Delegate declaration
    public delegate void MyDelegate(string message);

    // Method matching the delegate signature
    public static void DisplayMessage(string msg)
    {
        Console.WriteLine("Message: " + msg);
    }

    public static void Main()
    {
        // Instantiating delegate
        MyDelegate del = DisplayMessage;

        // Invoking delegate
        del("Hello from delegate!");
    }
}
```

但也可以像event那样多个加在一起.
```cs
public class MulticastDelegateDemo
{
    public delegate void Notify();

    public static void MethodA() => Console.WriteLine("Method A executed");
    public static void MethodB() => Console.WriteLine("Method B executed");

    public static void Main()
    {
        Notify notify = MethodA;
        notify += MethodB;

        notify(); // Invokes both MethodA and MethodB
    }
}
```

还可以换(geeks这个例子举得真好啊!)
```cs
public class ReturnDelegateDemo
{
    public delegate int Operation(int x, int y);

    public static int Add(int a, int b) => a + b;
    public static int Multiply(int a, int b) => a * b;

    public static void Main()
    {
        Operation op = Add;
        Console.WriteLine("Addition: " + op(5, 3));

        op = Multiply;
        Console.WriteLine("Multiplication: " + op(5, 3));
    }
}
```
