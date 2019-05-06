## 写在前面
最近在写[IQService](https://github.com/Lobster-King/IQService)这个模块间通信框架的时候，遇到了一个诡异的问题。虽然现在已经完美解决了，但是本着知其然且知其所以然的原则，现在把具体问题以及问题的解决方案还有背后的原理一一梳理出来。

## 问题描述

写完IQService核心功能之后，把IQServiceDemo运行起来程序直接挂掉了，崩在main函数里面，EXC____BAD____ACCESS野指针错误，而且控制台没有给出任何输出。  

![]()
