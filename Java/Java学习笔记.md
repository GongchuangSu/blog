# 基本语法知识

## 自增（前加加和后加加）

```java
int i = 0;
int j = 0;
System.out.println(i++);
System.out.println(i);
System.out.println(++j);
System.out.println(j);

/**
 * 输出结果：
 0
 1
 1
 1
*/
```

总结：

> 前加加：先加后用
>
> 后加加：先用后加

## `+=`运算符

The "common knowledge" of programming is that `x += y` is an equivalent shorthand notation of `x = x + y`. As long as `x` and `y` are of the same type (for example, both are `int`s), you may consider the two statements equivalent.

However, in Java, `x += y` *is not* identical to `x = x + y` in general.

If `x` and `y` are of different types, the behavior of the two statements differs due to the rules of the language. For example, let's have `x == 0` (int) and `y == 1.1` (double):

```java
    int x = 0;
    x += 1.1;    // just fine; hidden cast, x == 1 after assignment
    x = x + 1.1; // won't compile! 'cannot convert from double to int'
```

`+=` performs an implicit cast, whereas for `+` you need to explicitly cast the second operand, otherwise you'd get a compiler error.

## 线程间通信

### volatile关键字

> 关键字volatile可以用来修饰成员变量，就是告知程序任何对该变量的访问均需从共享内存中获取，而对它的改变必须同步刷新回共享内存，从而保证了所有线程对变量访问的可见性。

### synchronized关键字

> 关键字synchronized用来修饰方法或者以同步块的形式来进行使用，它主要确保多个线程在同一时刻，只有一个线程处于方法或同步块中，从而保证了线程对变量访问的可见性和排他性。

### 等待/通知机制

> 等待通知机制，是指一个线程A调用了对象O的wait()方法进入等待状态，而另一个线程B调用了对象O的notify()或notifyAll()方法，线程A收到通知后从对象O的wait()方法返回，进而执行后续操作。

### 管道输入/输出流

> 管道输入/输出流主要用于线程间的数据传输，而传输的媒介为内存。
>
> 对于Piped类型的流，必须先调用connect()方法进行绑定，否则会抛出异常。

### ThreadLocal的使用

> ThreadLocal，即线程变量，是一个以ThreadLocal对象为键，任意对象为值的存储结构，这个结构被附带在线程上，也就是说一个线程可以根据一个ThreadLocal对象查询到绑定在这个线程上的值。

## 多线程并发

### wait和sleep的区别

- sleep属于Thread类，wait属于Object
- 调用sleep()方法中，线程不会释放锁，到了指定时间会自动恢复运行；而调用wait()方法时，会释放对象锁，进入到等待队列，当此对象调用notify()或notifyAll()时会重新进入运行状态

## 进程和线程

可以从以下几个方面进行说明：

- 根本区别：进程是操作系统资源分配的基本单位，线程是任务调度执行的基本单位
- 内存分配：系统在运行时会为每个进程分配不同的内存空间，故进程间拥有独立内存；系统只为线程分配CPU，不分配内存，线程隶属于某一进程，且同一进程的各个线程间共享内存（资源）
- 调度切换：每个进程都有独立代码和数据空间（程序上下文），进程间切换姚保存上下文，加载另一个进程，消耗资源较大；而线程则共享了进程的上下文环境，每个线程都有独立的运行栈和程序计数器PC，切换开销小
- 通信方面：进程间相互独立，通信困难，常用方法：管道、信号、套接字、共享内存、消息队列等；线程间可以直接读写进程数据段（如全局变量）来进行通信——需要进程同步和互斥手段的辅助，以保证数据的一致性
- 包含关系：进程是系统的一个应用程序，线程是进程的一个任务