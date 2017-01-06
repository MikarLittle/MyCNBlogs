<h2>《Java 并发编程实战》 :books: </h2> 
> Brian Goetz、 Tim Peierls、 Joshua Bloch、 Joseph Bowbeer、 David Holmes、 Doug Lea 著   

```html
  1.什么是线程安全？
    当多个线程访问某个类时，不管运行时环境采用何种调度方式或者这些线程将如何交替执行，并且在主调代码中不需要任何额外的
  同步和协同，这个类都能表现出正确的行为，那么就称这个类是线程安全的。
    在线程安全类中封装了必要的同步机制，因此客户端无须进一步采取同步措施。
    无状态对象一定是线程安全的。
  
  2.原子性：
    ++count：“读取-修改-写入”的操作序列，其结果状态依赖之前的状态。
    竞态条件：最常见的竞态条件-“先检查后执行(Check-Then-Act)”操作，即通过一个可能失效的观测结果来决定下一步的动作。
    复合操作：“先检查后执行(Check-Then-Act)”和“读取-修改-写入”(例如递增运算)等操作统称为复合操作。
    假设有两个操作A和B，如果从执行A的线程来看，当另一个线程执行B时，要么将B全部执行完，要么完全不执行B，那么A和B对彼此
  来说是原子的。原子操作是指，对于访问同一个状态的所有操作(包括该操作本身)来说，这个操作是一个以原子方式执行的操作。
    在实际情况中，应尽可能地使用现有的线程安全对象(例如AcomicLong)来管理类的状态。与非线程安全的对象相比，判断线程安全
  对象的可能状态及其状态转换情况要更为容易，从而也更容易维护和验证线程安全性。
   
  3.加锁机制：
    要保持状态的一致性，就需要在单个原子操作中更新所有相关的状态变量。
    内置锁：静态的 synchronized 方法以 Class 对象作为锁。
    “重入”意味着获取锁的操作的粒度是“线程”，而不是调用。
  
  4.用锁来保护状态： 
    对于可能被多个线程同时访问的可变状态变量，在访问它时都需要持有同一个锁，在这种情况下，我们称状态变量是由这个锁保护的.
    每个共享的和可变的变量都应该只由一个锁来保护，从而使维护人员知道是哪一个锁。
    对于每个包含多个变量的不变性条件，其中涉及的所有变量都需要由同一个锁来保护。
  
  5.活跃性与性能：
    通常，在简单性与性能之间存在着相互制约因素。当实现某个同步策略时，一定不要盲目地为了性能而牺牲简单性(可能破坏安全性).
    当执行时间较长的计算或者可能无法快速完成的操作时(例如，网络 I/O 或控制台 I/O)，一定不要持有锁。
    
  6.可见性：
    在没有同步的情况下，编译器、处理器以及运行时等都可能对操作的执行顺序进行一些意想不到的调整。在缺乏足够同步的多线程
  程序中，要想对内存操作的执行顺序进行判断，几乎无法得出正确的结论。
    加锁与可见性：加锁的含义不仅仅局限于互斥行为，还包括内存可见性。为了确保所有线程都能看到共享变量的最新值，所有执行
  读操作或者写操作的线程都必须在同一个锁上同步。
    volatile 变量：仅当 volatile 变量能简化代码的实现以及对同步策略的验证时，才应该使用它们。如果在验证正确性时需要
  对可见性进行复杂的判断，那么就不要使用 volatile 变量。volatile 变量的正确使用方式包括：确保他们自身状态的可见性，
  确保它们所引用对象的状态的可见性，以及标识一些重要的程序生命周期事件的发生(例如：初始化或关闭)。
    加锁机制既可以确保可见性又可以确保原子性，而 volatile 变量只能确保可见性。
  
  7.发布与逸出：
    “发布(Publish)”一个对象是指使对象能够在当前作用域之外的代码中使用。
    当某个不应该发布的对象被发布时，这种情况就被称为逸出(Escape)。
    安全对象的构造过程：不要在构造过程中使用 this 引用逸出。
  
  8.线程封闭：  
    Ad-hoc 线程封闭：维护线程封闭性的职责完全由程序实现来承担。
    栈封闭：线程封闭的一种特例，在栈封闭中，只能通过局部变量才能访问对象。
    ThreadLocal类：它能使线程中的某个值与保存值的对象关联起来。
  
  9.不变性：
    不可变的对象一定是线程安全的。
    当满足以下条件时，对象才是不可变的：
      (1) 对象创建以后其状态就不能修改；
      (2) 对象的所有域都是 final 类型；
      (3) 对象是正确创建的(在对象创建期间，this 引用没有逸出)。
```