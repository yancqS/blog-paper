---
title: 堆和栈的区别
description: 堆和栈的区别
date: '2021-06-09'
cover: 'http://img.up-4ever.site/20210609120039.jpg'
top_img: 'http://img.up-4ever.site/infinity-9909115.jpg'
tags:
  - 计算机知识
  - 转载
  - 翻译
categories:
  - 计算机知识
abbrlink: 322da11e
---
# 堆和栈的区别

堆栈和堆之间的差异可能会让许多人感到困惑。所以，我认为整理一个关于堆栈问题和答案列表会非常有用。

## 堆和栈存储在哪里？

它们都存储在计算机的 RAM（随机存取存储器）中。

## 为什么要区分堆和栈？

之所以要区分堆和栈，是由于程序需要两种不同特性的内存形式而确定的。在C++中，新建一个对象有两种方式，**静态分配**和**动态分配**。

**静态分配**：

一般来说，静态分配用于初始化已知对象大小的时候，比如`int a[10];`如果我们能够确定这个数组是10个，我们可以使用这种方式。这种方式所需要的内存在编译期间即可确定，因此操作系统便可以预先确定所指定大小内存给变量，并且可以在变量生命周期结束后自动释放。

**动态分配**：

然而在某些场景下，可能需要根据某些情况来申请内存，比如`int* a =new int[count];`而变量`count`可能就来自于某个配置文件或者用户手动输入的值。这个时候操作系统就无法再预先确定内存大小，并且不执行到`new int[count]`这一行代码的时候，是无法知道所要分配的内存大小，因此操作系统分出一块内存，用来进行动态分配。并且规定，动态分配的内存需要由程序员自行管理。

>之前的[文章](http://up-4ever.site/posts/39d92d48/#Dynamic-allocation)也有所涉及。

## 堆和栈的区别
 - 堆是运行时确定内存大小，而栈在编译时即可确定内存大小。理由便是上一节中提到的，这是区分堆和栈的初衷。
 - 堆内存由用户管理，栈内存会被自动释放。
 - 栈实现方式采用数据结构中的栈实现，具有(LIFO)的顺序特点，堆为一块一块的内存（其数据结构更类似于链表）。
 - 栈由于其实现方式，在分配速度上比堆快的多。分配一块栈内存不过是简单的移动一个指针。
 - 栈为线程私有而堆为线程共享

虽然堆和栈有这么多的区别，但是这些区别都是由于操作系统而决定的，在硬件上，他们本质都是`RAM`。

## 线程如何与栈和堆交互？ 栈和堆如何在多线程中工作？

在多线程应用程序中，每个线程都会有自己的栈（内存）。但是所有的线程将共享堆（内存）。因为在多线程应用程序中，不同的线程共享堆（内存），这也意味着线程之间必须有一些协调策略，这样它们就不会试图同时访问和操作堆中的同一块内存。

>在之前的[线程安全](http://up-4ever.site/posts/f07340/)的文章中也曾提到。共享内存会带来线程安全问题。在JavaScript的共享工作者线程中曾提及这个问题，同样也是因为共享内存引起的。因此，JavaScript提供了Atomics对象来解决线程安全的问题。

## 为什么堆是线程共享的而栈不是？

上面最后一点提到了栈为线程私有而堆为线程共享。这是为什么呢？

其实很简单，为了解决一个问题：线程间通信。

想要实现线程间通信，目前有两种方法：

- 消息传递
- 共享内存

**共享内存**便是我们所说的将堆设置为线程间共享的，这样我们能够通过堆中的对象实现数据共享，这样便使得其他线程能够知道某个线程修改了某个数据。但是这样带来的问题可能就有线程安全问题等，但是这样做的优势便在于**速度更快和节约内存**，Java，C#等使用这种方式。

**消息传递**是每个线程都私有自己的数据空间，当需要线程通信的时候，便需要一个线程显示的给另外一个线程发送具体的消息，这样做的能够让多线程更加安全，但是私有的线程空间和消息传递带来的是需要给内个线程都拷贝相同的对象，变量等，并且可能会带来效率问题。而Erlang和JoCaml便是使用消息传递式线程通信。

## 栈内存与堆内存的生命周期？

一旦函数调用运行完成，栈区内存中为该函数调用创建的所有数据都将被自动删除。 堆区内存中的数据都将保留在那里，直到程序员手动删除为止。

>一些高级语言也有自动的GC机制，比如Java和JavaScript等。这个在之前的[文章](http://up-4ever.site/posts/39d92d48/)也有所提及。像C这样的语言，拥有底层内存管理原语，例如 malloc() 和 free()。开发人员使用这些原语显式地分配和释放操作系统中的内存（堆区内存）。

## 栈内存的大小能增加吗？堆内存的大小能增加吗？

栈内存被设置为固定大小，并且不能超过其固定大小（尽管有些语言的扩展允许这样做）。因此，如果栈区没有足够的空间来处理分配给它的内存，就会发生**栈溢出**。这通常发生在调用大量嵌套函数或者存在无限递归调用的时候。

如果堆区的当前大小太小，无法容纳新内存，那么操作系统可以向堆中添加更多内存。这是堆和堆栈之间的一大区别。

## 栈和堆是如何实现的？

栈和堆的实现实际上取决于语言、编译器和运行时。栈和堆的实现细节总是不同的，这取决于所使用的语言和编译器。但是，从总体来看，一种语言中的栈和堆用于完成与另一种语言中的栈和堆相同的事情。

## 哪个更快——栈还是堆？ 为什么？

栈由于其实现方式，在分配速度上比堆快的多。分配一块栈内存不过是简单的移动一个指针。

## 栈和堆上的内存是如何释放的？

当变量超出范围时(out of scope)，栈上的数据会自动释放。 但是，在 C 和 C++ 等语言中，存储在堆上的数据必须由程序员使用内置关键字之一（如 free、delete 或 delete[]）手动删除。 其他语言（如 Java 和 .NET）使用垃圾收集来自动从堆中删除内存，而无需程序员做任何事情。

## 栈和堆会出现什么问题？

栈内存不足时，会导致栈溢出，并可能导致程序崩溃。在堆内存可能存在内存碎片的问题。内存碎片产生的原因可参考[知乎-C语言内存碎片如何处理？](https://www.zhihu.com/question/51836333/answer/145693402)

>内存碎片即“碎片的内存”描述一个系统中所有不可用的空闲内存，这些碎片之所以不能被使用，是因为负责动态分配内存的分配算法使得这些空闲的内存无法使用，这一问题的发生，原因在于这些空闲内存以小且不连续方式出现在不同的位置。 ——[百度百科](https://baike.baidu.com/item/%E5%86%85%E5%AD%98%E7%A2%8E%E7%89%87/3883950)

## 该使用哪一个——栈还是堆？

因为栈很小，所以当你确切地知道数据需要多少内存时，或者如果知道数据需要的内存非常小时，就应该使用栈。当你知道数据需要大量内存，或者不确定需要多少内存（比如使用动态数组）时，最好使用堆。

## 参考文章
- [What’s the difference between a stack and a heap?](https://www.programmerinterview.com/data-structures/difference-between-stack-and-heap/)
- [堆和栈的区别](http://dengchengchao.com/?p=922)