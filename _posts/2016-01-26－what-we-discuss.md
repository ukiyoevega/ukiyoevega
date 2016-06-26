---
layout: archive
title: 开会笔记
---

1. MVC向MVVM的转换 (什么是ReactiveCocoa)

    ReactiveCocoa是由Github开源的一个应用于iOS和OS X开发的新框架。

    受函数响应式编程激发，不同于使用可变的变量替换和就地修改，RAC提供Signals（被表示为RACSignal）来捕获当前值和将来值。通过链接（chaining），组合（combining）和对Signals做出反应（reacting），我们不必频繁地观察并更新值，而是声明式编写软件。

    RAC一个重要的优点就是它提供了单独的、统一的方法来处理异步行为，包括委托方法，回调blocks，target-action机制，通知和KVO。

    MVC是模型Model-View-Controller的设计模式。Model包含了数据以及对数据的操作，View包含了软件中可视化的部分，而Controller用于关联Data Model和View对象。，Model类代表原始数据，例如文档、设置、文件、内存中的对象等，View是模型中数据的可视化表现，而Controller类则包含了将模型和其对应视图连接起来的逻辑，并保持前二者的状态同步。

    MVVM代表的是Model-View-ViewModel。MVVM 利用双向绑定技术，使得 Model 变化时，ViewModel 会自动更新，而 ViewModel 变化时，View 也会自动变化。所以，MVVM 模式有些时候又被称作Model-View-Binder模式。具体在 iOS 中，可以使用 KVO 或 Notification 技术达到这种效果。


2. 什么时候用到interface，什么时候用到struct？



3. 从前的Objective-C是面向对象(oo)语言，Swift为何向(op)转型？

    面向协议编程是在面向对象编程基础上演变而来，将程序设计过程中遇到的数据类型的抽象由使用基类进行抽取改为使用协议进行抽取。

    面向协议的编程的核心是抽象（abstraction）和简化（simplicity）。可以让代码更加清晰，隔离性更好，可以重新组合方法定义的位置，并把泛型抽取到定义扩展时，从而使得方法的定义更加清晰，也更加美观。
    面向对象编程和面向协议编程最明显的区别在于程序设计过程中对数据类型的抽取（抽象）上，面向对象编程使用类和继承的手段，数据类型是引用类型；而面向协议编程使用的是遵守协议的手段，数据类型是值类型（Swift中的结构体或枚举）。

4. 了解Clang编译器，什么是LLVM？

传统的编译器通常分为三个部分，前端(frontEnd)，优化器(Optimizer)和后端(backEnd)。在编译过程中，前端主要负责词法和语法分析，将源代码转化为抽象语法树；优化器则是在前端的基础上，对得到的中间代码进行优化，使代码更加高效；后端则是将已经优化的中间代码转化为针对各自平台的机器代码。

Clang 是一个C、C++、Objective-C和Objective-C++编程语言的编译器前端。它采用了底层虚拟机作为其后端。它的目标是提供一个GNU编译器套装的替代品。
优点：编译速度快，内存消耗小，编译错误可读性强（出错提示友好），兼容GCC，能对代码进行静态分析，代码组织结构清晰，License是BSD。


5. ARC和MRC的区别？

    Swift 使用自动引用计数(ARC)机制来跟踪和管理你的应用程序的内存。通常情况下，Swift 的内存管理机制会一直起着作用，你无须自己来考虑内存的管理。ARC 会在类的实例不再被使用时，自动释放其占用的内存。

    当你每次创建一个类的新的实例的时候，ARC会分配一大块内存用来储存实例的信息。内存中会包含实例的类型信息，以及这个实例所有相关属性的值。当实例不再被使用时，ARC 释放实例所占用的内存，并让释放的内存能挪作他用。这确保了不再被使用的实例，不会一直占用内存空间。当 ARC 收回和释放了正在被使用中的实例，该实例的属性和方法将不能再被访问和调用。

    为了确保使用中的实例不会被销毁，ARC 会跟踪和计算每一个实例正在被多少属性，常量和变量所引用。哪怕实例的引用数为1，ARC都不会销毁这个实例。因此无论你将实例赋值给属性、常量或变量，它们都会创建此实例的强引用。之所以称之为“强”引用，是因为它会将实例牢牢的保持住，只要强引用还在，实例是不允许被销毁的。

    在MRC的内存管理模式下，与对变量的管理相关的方法有：retain,release和autorelease。retain和release方法操作的是引用记数，当引用记数为零时，便自动释放内存。并且可以用NSAutoreleasePool对象，对加入自动释放池（autorelease调用）的变量进行管理，当drain时回收内存。

    retain方法的作用是将内存数据的所有权附给另一指针变量，引用数加1，即retainCount+= 1;

    release方法是释放指针变量对内存数据的所有权，引用数减1，即retainCount-= 1;

    autorelease方法是将该对象内存的管理放到autoreleasepool中。


6. UITableView的渲染顺序是怎样的？






7. Silver是什么？

    Silver编译器能让开发者可以直接使用Swift语言来编写iOS、Android、Windows应用。Silver编译器可以为.NET CLR、Java/Android JVM和Cocoa运行时提供编译工作。尽管Silver支持iOS、Android和Windows三大平台，但它却有着非常明确的非跨平台方向定位，致力于让开发者可以在各个平台上利用Swift语言以原生的方式来构建应用。
    Silver的开发公司RemObjects认为，用户界面应用原生开发，iOS应用应该用iOS的用户界面库才会让人觉得在iOS上最合适，.NET应用应该用微软的Windows用户界面库，Java应用应该用Android或Java库。因此，对于在Windows从事开发工作的开发者，Silver深度集成了Visual Studio 2013和2015 IDE，而对于Mac开发者，Silver则为其带来了自主研发的Fire开发环境。


8. CMake。



9. Node.js。

Node.js是一个基于Chrome V8引擎的JavaScript运行环境。Node.js 使用了一个事件驱动、非阻塞式I/O的模型，使其轻量又高效。Node.js 的包管理器npm，是全球最大的开源库生态系统。

10. Socket.IO。

    Socket.IO是一个WebSocket库，包括了客户端的js和服务器端的nodejs，它的目标是构建可以在不同浏览器和移动设备上使用的实时应用。它会自动根据浏览器从WebSocket、AJAX长轮询、Iframe流等等各种方式中选择最佳的方式来实现网络实时应用，非常方便和人性化，而且支持的浏览器最低达IE5.5，应该可以满足绝大部分需求了。
    Socket实现实时、双向的、基于事件的通信，能够在不同的平台、浏览器、设备上使用，注重Socket.io将Websocket和轮询（Polling）机制以及其它的实时通信方式封装成了通用的接口，并且在服务端实现了这些实时机制的相应代码。

11. lighter view controller 