## 写在前面
最近在写[IQService](https://github.com/Lobster-King/IQService)这个模块间通信框架的时候，遇到了一个诡异的问题。虽然现在已经完美解决了，但是本着知其然且知其所以然的原则，现在把具体问题以及问题的解决方案还有背后的原理一一梳理出来。

## 问题描述

写完IQService核心功能，把IQServiceDemo运行起来之后，程序直接挂掉了。程序崩在main函数里面，EXC____BAD____ACCESS野指针错误，而且控制台没有给出任何输出。  

![](https://github.com/Lobster-King/AppArticles/raw/master/iOS%20Tips/tip1_1.jpeg)  

## 解决问题
EXC____BAD____ACCESS错误我们知道是访问了野指针，我们一般可以通过打开XCode->Edit Scheme->开启Zomie Objects来监控捕捉野指针地址。如下图。

![](https://github.com/Lobster-King/AppArticles/raw/master/iOS%20Tips/tip1_2.jpeg)  

打开Zomie Objects之后，重新run，发现控制台输入了所访问异常的野指针地址。具体报错信息为-[CFString release]: message sent to deallocated instance 0x600003712d40。  

![](https://github.com/Lobster-King/AppArticles/raw/master/iOS%20Tips/tip1_3.jpeg) 

这时候我似乎发现了问题所在，在IQService中，我利用NSInvocation进行了方法的调用，并通过调用NSInvocation的getReturnValue函数获取返回值，其返回值恰好是个NSString，难道NSString被提前释放了？  

![](https://github.com/Lobster-King/AppArticles/raw/master/iOS%20Tips/tip1_4.jpeg)  

```
    /*发生问题的代码*/
    id returnValue = nil;
    
    NSInteger index = 2;
    for (id arg in argsArray) {
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wincompatible-pointer-types-discards-qualifiers"
        [invocation setArgument:&arg atIndex:index];
#pragma clang diagnostic pop
        index++;
    }
    [invocation invokeWithTarget:instance];
    if (mSignature.methodReturnLength) {
        [invocation getReturnValue:&returnValue];
        NSLog(@"%p",returnValue);
    }
    return returnValue;
```

## 问题本质
* NSInvocation类中getReturnValue函数，其入参接受的是void*类型（指针类型）。  
* 我们声明的变量如果没有修饰符限定的话，默认都是**__strong**修饰的。  
* getReturnValue函数传入的是一个指针，它只是把传入的指针指向了返回值（NSString），也就是说没有Retain这个返回值（NSString）。  
* 出了getReturnValue函数作用域，给NSString对象发送release不会报错，而出了invokeService:(NSString *)service withArgs:(va_list)arguments函数作用域，回收栈内存的时候依然给栈内存所指向的也就是NSString这个实例发送了release消息，导致过度释放！出现了野指针错误。
* 程序依然能正常返回是因为NSString对象回收后，操作系统没有立即覆盖这个内存地址。