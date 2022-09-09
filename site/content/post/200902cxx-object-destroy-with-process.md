---
layout: default
title: 'C++ 对象是怎么死的&#65311;进程篇'
date: None
author:
    display_name: 
---

　　我承认这个帖子的名称有标题党的嫌疑，但是暂时想不出更好的名称了，只好先这样了 :-(  
　　由于前天的帖子聊了[架构设计的多进程问题](https://program-think.blogspot.com/2009/02/multi-process-vs-multi-thread.html)，所以今天想起来要聊一下与“C++进程终止”相关的那些事。与前几个 C++ 帖子的风格类似，今天聊的内容，尽量局限于标准 C++ 范畴，尽量不涉及特定的操作系统平台。 　　由于今天讲的是“进程篇”，自然得先搞明白进程的几种死法。其实进程和大活人一样，也有三种死法，分别是“自然死亡、自杀、它杀”。这三种死亡方式具体如下： 　　望文生义，自然死亡就是最自然的进程退出方法。具体表现为通过return语句结束main函数。由于这种方法最优雅（后面会说），如果没有其它特殊原因，强烈建议采用这种死法。  
　　所谓的自杀，就是进程自己调用某些 API 来自行了断。在标准 C++ 中，这几个函数（exit、abort、terminate、unexpected）可以用于进程自杀。如果没有额外设置，unexpected 函数默认会调用 terminate 函数，terminate 函数默认会调用 abort 函数。所以自杀的方式基本上也就是 exit 和 abort 两种。exit 相对 abort 来说温和一些，所以下文称 exit 为**温和自杀**；相对地，把 abort 称为**激进自杀**。 　　它杀其实也挺好理解，就是当前进程被其它进程杀死。标准 C++ 没有提供用于它杀的 API 函数，因此常用的方法是通过某些跨平台的库（如 ACE）提供的 API 函数或者调用某些外部命令（如 Posix 系统的 kill 命令）来实现。 　　上面说了这几种死法，有同学要问了：进程不同的死法和 C++ 对象有什么关系捏？其实关系大大滴，请听我细细道来。  
　　首先把类对象分为三种：局部非静态对象、局部静态对象、非局部对象（出于习惯，以下简称**全局对象**）。对于尚不清楚这几种对象差异的同学，请先找本 C++ 入门书拜读一下。 　　进程不同的死法对于这几种对象是否能销毁会有很大的影响。请看如下的对照表： －－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－ ｜　　　　　｜　局部非静态对象　｜　局部静态对象　｜　全局对象　｜ ｜自然死亡　｜　能　　　　　　　｜　能　　　　　　｜　能　　　　｜ ｜温和自杀　｜　不能　　　　　　｜　能　　　　　　｜　能　　　　｜ ｜激进自杀　｜　不能　　　　　　｜　不能　　　　　｜　不能　　　｜ ｜它杀　　　｜　不能　　　　　　｜　不能　　　　　｜　不能　　　｜ －－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－－ 　　从这个对照表可以看出，激进自杀和它杀的效果类似（各种类对象都【无法】正常销毁）。所以我们在写程序时要极力避免上述这两种情况。

　　另外，温和自杀也有不爽之处：不能正确地销毁局部非静态对象。准确地说，应该是：在调用exit之前已经构造但是尚未析构的**局部非静态**对象将再也不会被析构。所以温和自杀也要避免使用。

　　综上所述，最正经、最靠谱的死法就是第一种：自然死亡。 　　那么，是不是只要让进程自然死亡就万事大吉了？非也！即使所有的类对象都会被析构，还有另一个棘手的问题：析构的顺序。 　　先来看下面一个例子：

class CFoo
{
public: CFoo() { cout << "CFoo" << endl; } virtual ~CFoo() { cout << "~CFoo" << endl; }
};

　　上述示例挺简单的（有效代码仅6行），大伙儿能看出有什么问题吗？（如果你一眼就看出问题之所在，恭喜你——你的 C++ 水平够高，后面的内容你不用看了） 　　对于用户定义的全局对象，在 C++ 标准中并【没有】规定它们构造和析构的先后顺序；对于诸如标准输入输出流的 cout、cerr 等全局对象，在 C++ 03 标准中（27.4.2.1.6章节）有提及如何保证它们在最后析构。但由于某些老式编译器并未完全遵照标准实现，导致标准输入输出流的几个全局对象【有可能】被提前析构。

　　基于上述原因，假如 CFoo 类也定义了一个全局对象 g\_foo。当 g\_foo 析构的时候，cout 对象【有可能】已经先死了（取决于具体的环境，详见《[关于标准输入输出流的进一步探讨](https://program-think.blogspot.com/2009/02/cxx-object-destroy-with-io-stream.html)》）。在这种情况下，CFoo 析构函数的打印语句由于引用了已死的对象，可能会导致【不可预料的后果】（比如进程崩溃）。

　　从上面的例子可以看出，如果你在程序中使用了全局对象或者静态对象，那你要非常小心地编写相关 class/struct 的析构函数代码，尽量【不要】在它们的析构函数中引用其它的全局对象或静态对象。当然啦，假如能避免使用全局对象和静态对象，就更好了。 　　另外，在 C++ 经典名著《Modern C++ Design》的第6章详细描述了关于单键/单例（Singleton）销毁的一些细节、场景及解决方法。大伙儿可以去拜读一下。

　　下一个帖子，咱们来聊一下[和线程有关的C++对象是怎么死的](https://program-think.blogspot.com/2009/03/cxx-object-destroy-with-thread-win32.html)。
