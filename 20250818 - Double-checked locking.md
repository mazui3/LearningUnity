隨手 Design Pattern (5) - 雙重檢查鎖定模式 (Double-Checked Locking Pattern)
>https://raychiutw.github.io/2019/%E9%9A%A8%E6%89%8B-Design-Pattern-5-%E9%9B%99%E9%87%8D%E6%AA%A2%E6%9F%A5%E9%8E%96%E5%AE%9A%E6%A8%A1%E5%BC%8F-Double-Checked-Locking-Pattern/

Double-checked locking
>https://en.wikipedia.org/wiki/Double-checked_locking

读wiki感觉double checked locking跟singleton是两个东西。\
但在前辈的代码里读到的是下面这个ai gen出来的答案类似。\
以double checked locking的方式来完成？加固？singleton这个pattern？\
有空再好好看一下。\
以及ai还说了volatile这个keyword，前辈的代码也妹说呐。

Double-Checked Locking (DCL) in C# is a design pattern used to implement thread-safe lazy initialization, typically for Singleton patterns, while minimizing the overhead of locking.\
The core idea of DCL involves:

- First Check (without lock):
A check is performed to see if the instance has already been created. If it exists, the method returns the existing instance without acquiring a lock, which improves performance in the common case where the instance is already initialized.
- Acquire Lock (if null):
If the instance is null, a lock is acquired to ensure only one thread proceeds to create the instance.
- Second Check (within lock):
Inside the locked section, a second check is performed to ensure that another thread hasn't already created the instance while the current thread was waiting for the lock.
- Instance Creation:
If the instance is still null after the second check, it is then created and assigned.
```
public sealed class Singleton
{
    private static volatile Singleton _instance; // volatile keyword is crucial
    private static readonly object _lock = new object();

    private Singleton() { } // Private constructor to prevent external instantiation

    public static Singleton Instance
    {
        get
        {
            if (_instance == null) // First check (without lock)
            {
                lock (_lock) // Acquire lock
                {
                    if (_instance == null) // Second check (within lock)
                    {
                        _instance = new Singleton(); // Instance creation
                    }
                }
            }
            return _instance;
        }
    }
}
```

Importance of volatile:\
The volatile keyword is crucial in DCL in C# because it prevents compiler and CPU optimizations that could reorder memory operations. \
Without volatile, a partially constructed object might be visible to other threads, leading to race conditions and incorrect behavior. volatile ensures that reads and writes to the _instance variable are always performed directly from main memory, preventing stale values from being read from CPU caches.
