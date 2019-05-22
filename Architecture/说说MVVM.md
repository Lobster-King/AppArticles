## 写在前面  
关于软件架构模式（确切的说是一种软件编码规范或者软件开发模式），这几年骂战不断。争论的焦点主要是在MVC、MVVM、MVP哪种架构最好，哪种架构才是最牛逼的、扩展性更强的、可维护性更高的。笔者不才，在实际项目中很少用过MVP架构，对于MVP的掌握也是只停留在写写Demo阶段。本篇文章主要着重介绍下MVVM架构在真实项目当中的应用，以及抛开RAC，我们如何自己动手写一个View和ViewModel之间的绑定框架。  

## MVVM扫盲  
>MVVM（Model–View–Viewmodel）是一种软件架构模式。

>MVVM有助于将图形用户界面的开发与业务逻辑或后端逻辑（数据模型）的开发分离开来，这是通过置标语言或GUI代码实现的。MVVM的视图模型是一个值转换器， 这意味着视图模型负责从模型中暴露（转换）数据对象，以便轻松管理和呈现对象。在这方面，视图模型比视图做得更多，并且处理大部分视图的显示逻辑。 视图模型可以实现中介者模式，组织对视图所支持的用例集的后端逻辑的访问。

>MVVM是马丁·福勒的PM（Presentation Model）设计模式的变体。 MVVM以相同的方式抽象出视图的状态和行为,但PM以不依赖于特定用户界面平台的方式抽象出视图（创建了视图模型）。
MVVM和PM都来自MVC模式。

>MVVM由微软架构师Ken Cooper和Ted Peters开发，通过利用WPF（微软.NET图形系统）和Silverlight（WPF的互联网应用派生品）的特性来简化用户界面的事件驱动程序设计。 微软的WPF和Silverlight架构师之一John Gossman于2005年在他的博客上发表了MVVM。

>MVVM也被称为model-view-binder，特别是在不涉及.NET平台的实现中。ZK（Java写的一个Web应用框架）和KnockoutJS（一个JavaScript库）使用model-view-binder。  

以上内容均来自维基百科。[MVVM wikipedia](https://zh.wikipedia.org/wiki/MVVM)。  

简单的讲，MVVM是MVC的改进版。我们都知道MVC软件架构模式是苹果推荐的开发模式。

**MVC**中的**M**就是单纯的从网络获取回来的数据模型，**V**指的我们的视图界面，而**C**就是我们的ViewController。

在其中，ViewController负责View和Model之间调度，View发生交互事件会通过target-action或者delegate方式回调给ViewController，与此同时ViewController还要承担把Model通过KVO、Notification方式传来的数据传输给View用于展示的责任。***随着业务越来越复杂，视图交互越复杂，导致Controller越来越臃肿，负重前行。脏活累活都它干了，到头来还一点不讨好。福报修多了的结果就是，不行了就重构你，重构不了就换掉你。***😅

来一张斯坦福老头经典的MVC架构图。  

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/Architecture/mvc-arch.jpg)  

所以为了解决这个问题，MVVM就闪亮登场了。他把View和Contrller都放在了View层（相当于把Controller一部分逻辑抽离了出来），Model层依然是服务端返回的数据模型。***而ViewModel充当了一个UI适配器的角色，也就是说View中每个UI元素都应该在ViewModel找到与之对应的属性。除此之外，从Controller抽离出来的与UI有关的逻辑都放在了ViewModel中，这样就减轻了Controller的负担。***  

我简单的画了下MVVM的架构图。  

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/Architecture/mvvm-arch.png)   

从以上的架构图中，我们可以很清晰的梳理出各自的分工。  

* **View层**：视图展示。包含UIView以及UIViewController，View层是可以持有ViewModel的。
* **ViewModel层**：视图适配器。暴露属性与View元素显示内容或者元素状态一一对应。一般情况下ViewModel暴露的属性建议是readOnly的，至于为什么，我们在实战中会去解释。还有一点，ViewModel层是可以持有Model的。
* **Model层**：数据模型与持久化抽象模型。数据模型很好理解，就是从服务器拉回来的JSON数据。而持久化抽象模型暂时放在Model层，是因为MVVM诞生之初就没有对这块进行很细致的描述。按照经验，我们通常把数据库、文件操作封装成Model，并对外提供操作接口。（有些公司把数据存取操作单拎出来一层，称之为**DataAdapter层**，所以在业内会有很多MVVM的变种，但其本质上都是MVVM）。
* **Binder**：MVVM的灵魂。可惜在MVVM这几个英文单词中并没有它的一席之地，它的最主要作用是在View和ViewModel之间做了双向数据绑定。如果MVVM没有Binder，那它与MVC无异。

我们发现，正是因为View、ViewModel以及Model间的清晰的持有关系，所以在三个模块间的数据流转有了很好的控制。


**这里给大家推荐一篇博文[猿题库iOS客户端架构设计](http://www.cocoachina.com/ios/20160108/14911.html)，其架构图如下。**  

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/Architecture/%E7%8C%BF%E9%A2%98%E5%BA%93Arch.png)  

猿题库的架构本质上不是MVC也不是MVVM，它是两种架构演进的一种架构模式。博文中对于MVC和MVVM的优缺点做了简单的介绍。  

* MVC缺点：Massive View Controller，也就是胖VC。
* MVVM缺点：1.学习成本高。2.DEBUG困难。

但博文中关于MVVM的阐述有两处笔者不太赞同。  

* MVVM绝不等于RAC，所以MVVM并不存在DEBUG难的问题。  
* MVVM正是因为跟RAC不对等，所以博文中“MVVM一个首要的缺点是，MVVM的学习成本和开发成本都很高”这句话也是不成立的。  

MVVM架构本身并不复杂，而且不用RAC我们依然可以通过KVO、类KVO的方式来帮我们实现View和ViewModel绑定器功能。  

关于猿题库iOS客户端架构设计是否合理，因为笔者不了解其具体业务，所以不能妄下结论。但是有一点可以肯定的是，**MVVM ≠ RAC**。

**一年一度的QA环节来了。**  
>Q：View和ViewModel之间是否一定要解耦？  
>A：View持有ViewModel，ViewModel不能持有View（即ViewModel不能依赖UIKit中任何东西）。说明白了吧？😅  解耦是有一定成本的，不管是通过Category或者中间件，消息链条都会无形之中变长，会有一定的DEBUG成本。  
>

## MVVM结合RAC
  
[ReativeCocoa](https://github.com/ReactiveCocoa/ReactiveCocoa)相信大家并不陌生，这个函数响应式框架在Github中已经有将近2w star 。RAC是个非常优秀的框架，它可以独立于MVVM而存在。**如果只是把它理解成MVVM中View和ViewModel Binder角色的话，那就有点大材小用了。**本文不会对RAC进行展开分析，感兴趣的可以自行实践一下。

RAC特点：  

* 语法怪异，杂交种。（函数式+响应式编程组合）
* 万物皆可盘。（事件信号RACSignal贯穿整个框架）
* 把离散的函数调用撺成一坨💩。（个人感觉跟Promise很像）

**总结：RAC是一种编程思维的改变，所以其缺点很明显，学习成本很大！！！**  

下面以登录模块举例子，简单介绍下MVVM+RAC的实现Demo。 







## MVVM结合非RAC（[IQDataBinding](https://github.com/Lobster-King/IQDataBinding)）

通过MVVM扫盲部分，我们了解到，Binder在MVVM中扮演了View和ViewModel数据通信者的角色。

了解过Android开发的同学都知道，Java有个好东西，那就是**注解（Annotation）**。在开发Android App的时候，可以在XML中通过标记的方式告诉编译器与ViewModel的绑定关系。编译器在编译过程中，会自动生成XML和ViewModel的绑定类（Binder）。

注解功能很强大，但是不幸的是，我们iOS（Objective-C）没有！！！Swift有没有注解笔者不太清楚，有知道的童鞋可以告诉我一下。  

接下来我们将一步步实现一个View和ViewModel双向绑定的框架。  


**方案一：“躺爽法”**

>名次解释：所谓“躺爽法”（实在想不出用什么词描述这种最基础的方法了😅）和KVO，是相对于ViewModel >>> View而言的。

1.ViewModel >>> View：View不需要关心ViewModel属性的改变，View只需要提供更新视图的接口即可，ViewModel属性改变之后调用View提供的API更新视图。所以View这里没有做过多的事情，一切都是被动触发，所以我称作是“躺爽法”。  

2.View >>> ViewModel：用户操作视图，比如一个开关按钮，这时候要同步给ViewModel。我们知道View是可以持有ViewModel的，所以在View中我们可以直接拿到ViewModel指针，进而通过ViewModel暴露的更新方法而更新值。  

>高能预警：这种最基础的方法，实际上是MVC！！！他本身没有解决**“Massive View Controller”**问题。也就是说为了ViewModel中不依赖于View，必须通过Controller中转，依然会有一堆胶水代码。**所以这种解决方案并不是MVVM！！！**不是故意给大家挖坑，只是意在提醒大家，阅读文章的时候要举一反三，更不要被一些脏乱差的文章混淆视听😅😅😅。  


**方案一：KVO**  

1.ViewModel >>> View：ViewModel属性改变之后，通知View进行视图布局。这种最熟悉不过，通过KVO即可实现。  

2.View >>> ViewModel：用户操作视图，通过ViewModel暴露的更新方法而更新值（设置属性值时要避开触发KVO监听，否则会出现死循环）。  

**Talk is cheap,show me the code!**  
我们以大家最熟悉的Cell举例子。  
**ViewModel**

```
//
//  IQMVVMDemoViewModel.h
//
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface IQMVVMDemoViewModel : NSObject

@property (nonatomic, copy, readonly) NSString *userName;
@property (nonatomic, copy, readonly) NSString *userPwd;

+ (IQMVVMDemoViewModel *)demoViewWithName:(NSString *)userName withPwd:(NSString *)userPwd;
- (void)updateViewModelWithName:(NSString *)userName withPwd:(NSString *)userPwd;

@end

NS_ASSUME_NONNULL_END

```

```
//
//  IQMVVMDemoViewModel.m
//  

#import "IQMVVMDemoViewModel.h"

@interface IQMVVMDemoViewModel ()

@property (nonatomic, copy, readwrite) NSString *userName;
@property (nonatomic, copy, readwrite) NSString *userPwd;

@end

@implementation IQMVVMDemoViewModel

+ (IQMVVMDemoViewModel *)demoViewWithName:(NSString *)userName withPwd:(NSString *)userPwd {
    IQMVVMDemoViewModel *viewModel = [[IQMVVMDemoViewModel alloc]init];
    viewModel.userName  = userName;
    viewModel.userPwd   = userPwd;
    return viewModel;
}

- (void)updateViewModelWithName:(NSString *)userName withPwd:(NSString *)userPwd {
    _userName   = userName;
    _userPwd    = userPwd;
}

@end
```

**View**

```
//
//  IQMVVMDemoView.h
//  
#import <UIKit/UIKit.h>

NS_ASSUME_NONNULL_BEGIN

@class IQMVVMDemoViewModel;

@interface IQMVVMDemoView : UITableViewCell

- (void)updateViewWithViewModel:(IQMVVMDemoViewModel *)viewModel;

@end

NS_ASSUME_NONNULL_END
```

```
//
//  IQMVVMDemoView.m
//  

#import "IQMVVMDemoView.h"
#import "IQMVVMDemoViewModel.h"

@interface IQMVVMDemoView ()<UITextFieldDelegate>

@property (nonatomic, strong) UITextField *userNameField;
@property (nonatomic, strong) UITextField *userPwdField;
@property (nonatomic, strong) IQMVVMDemoViewModel *viewModel;

@end

@implementation IQMVVMDemoView

#pragma mark--Life Cycle--
- (void)dealloc {
    [self.viewModel removeObserver:self forKeyPath:@"userName"];
    [self.viewModel removeObserver:self forKeyPath:@"userPwd"];
}

- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(NSString *)reuseIdentifier {
    if (self = [super initWithStyle:style reuseIdentifier:reuseIdentifier]) {
        [self setupSubviews];
    }
    return self;
}

#pragma Public & Private Methods--
- (void)setupSubviews {
    [self.contentView addSubview:self.userNameField];
    [self.contentView addSubview:self.userPwdField];
    /*
     这里做布局，不写了啊
     */
}

- (void)updateViewWithViewModel:(IQMVVMDemoViewModel *)viewModel {
    self.viewModel = viewModel;
    [self.viewModel addObserver:self forKeyPath:@"userName" options:NSKeyValueObservingOptionNew context:NULL];
    [self.viewModel addObserver:self forKeyPath:@"userPwd" options:NSKeyValueObservingOptionNew context:NULL];
}

#pragma mark--Delegates & KVO--
- (void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context {
    if ([keyPath isEqualToString:@"userName"]) {
        self.userNameField.text = change[NSKeyValueChangeNewKey];
    } else if([keyPath isEqualToString:@"userPwd"]) {
        self.userPwdField.text = change[NSKeyValueChangeNewKey];
    }
}

- (void)textFieldDidEndEditing:(UITextField *)textField {
    /*更新ViewModel*/
    if (textField == self.userNameField) {
        self.userNameField.text = textField.text;
    } else {
        self.userPwdField.text = textField.text;
    }
    [self.viewModel updateViewModelWithName:self.userNameField.text withPwd:self.userPwdField.text];
}

#pragma mark--Getters & Setters--
- (UITextField *)userNameField {
    if (!_userNameField) {
        _userNameField = [[UITextField alloc]init];
        _userNameField.delegate = self;
    }
    return _userNameField;
}

- (UITextField *)userPwdField {
    if (!_userPwdField) {
        _userPwdField = [[UITextField alloc]init];
        _userPwdField.delegate = self;
    }
    return _userPwdField;
}


@end

```

至此，我们大致把View和ViewModel之间数据通信方式给理清了。但是大家都知道KVO存在各种问题，而且每次监听一个属性都要写大量的代码（注册、移除、收到监听的处理）。所以方案一存在以下问题：  

* 直接使用KVO方式，每次都要写大量的注册、移除等代码，没有做到自动移除。
* 如果没有移除监听可能直接导致Crash，使用姿势不方便。

**方案二：类KVO（IQDataBinding）**

>名词解释：之所以称之为类KVO，是因为方案二本质上是通过KVO来实现的。不过IQDataBinding实现了自动移除，且支持函数式、链式调用，使用姿势比较优雅。  

空说无凭，我们来看看IQDataBinding如何使用  

**Controller**

```
/*引入NSObject+IQDataBinding头文件*/
- (void)configData {
    self.contentModel = [[ContentModel alloc]init];
    self.contentModel.title = @"lobster";
    self.contentModel.content = @"123456";
    
    /*View和ViewModel之间绑定*/
    [self.contentView bindModel:self.contentModel];
    
}

```

**View**

```
/*ViewModel >>> View*/
- (void)setUpSubviews {
    

    [self addSubview:self.loginTextField];
    [self addSubview:self.pwdTextField];
    
    self.loginTextField.frame = CGRectMake(0, 0, self.bounds.size.width, 30);
    self.pwdTextField.frame = CGRectMake(0, 40, self.bounds.size.width, 30);
    
    /*绑定ViewModel中title和content属性，发生改变自动触发View更新操作*/
    __weak typeof(self)weakSelf = self;
    self.bind(@"title",^(id value){
        weakSelf.loginTextField.text = value;
    }).bind(@"content",^(id value){
        weakSelf.pwdTextField.text = value;
    });
    
}
    
```
```
/*View >>> ViewModel*/
- (BOOL)textFieldShouldReturn:(UITextField *)textField {
    [textField resignFirstResponder];
    if (textField.text) {
        /*函数式调用*/
        self.update(@"content",textField.text).update(@"title",@"lobster");
    }
    return YES;
}
```

**IQDataBinding踩坑记：**

* View更新ViewModel属性时，如何让一个函数支持传输不同的数据类型？
* View更新ViewModel时，如何避免触发KVO而导致死循环？
* 如何自动移除KVO？

**View更新ViewModel属性时，如何让一个函数支持传输不通的数据类型？**  

笔者借鉴了Masonry框架的解决方案，通过宏定义+不定参数解决了传输不通数据类型的问题。感兴趣的可以了解下Masonry中_MASBoxValue这个函数。  

**View更新ViewModel时，如何避免触发KVO而导致死循环？**  

很显然，通过setValue:forKey:函数会触发KVO回调，所以我的解决方案是获取到IVar，直接设置实例变量的值。但是object_setIvar(id _Nullable obj, Ivar _Nonnull ivar, id _Nullable value) 函数，只接收id类型的值。Stack Overflow查询之后，发现可以通过函数类型强转的方式来解决。

**如何自动移除KVO？**  

这个问题就比较简单了，为了监控View的Dealloc函数调用时机，我们可以通过Hook的方式，但是Hook不太推荐。尤其使用类似于Aspects（通过消息转发来实现，代价很高）进行Hook时，对于那种一秒钟调用超过1000次的业务场景会严重影响性能。所以我才用的方案是，通过给View添加一个关联对象来解决。因为我们知道对象释放时会先释放成员变量，然后再释放关联对象，所以我们可以在关联对象的dealloc方法里对观察者进行自动移除。  

回忆下对象的释放过程  

```
/*对象在释放时，最终都会走到这个函数*/
void *objc_destructInstance(id obj) 
{
    if (obj) {
        // Read all of the flags at once for performance.
        bool cxx = obj->hasCxxDtor();
        bool assoc = obj->hasAssociatedObjects();

        // This order is important.
        if (cxx) object_cxxDestruct(obj);/*如果有成员变量，则先释放成员变量*/
        if (assoc) _object_remove_assocations(obj);/*如果有关联对象，则释放关联对象*/
        obj->clearDeallocating();/*清除SideTable中weak引用表，并把指向该对象的指针置为nil*/
    }

    return obj;
}
```

**GitHub地址：[IQDataBinding，一个View和ViewModel双向绑定的框架](https://github.com/Lobster-King/IQDataBinding)**

## 对于开发者的建议

对于团队成员不是很多，项目又比较新的团队来说，我建议大家勇于尝试MVVM架构。  
而对于团队成员众多，项目遗留问题多的团队来说，我建议大家尝试MVVM+KVO+数据存取放到Model层的架构方案。其次，建议大家先尝试下猿题库的架构模式。猿题库的架构模式上手比较简单，但是Controller中也承担了View和ViewModel之间交互的职责，而且架构执行过程中也极易越写越偏，有可能可能最中写的代码是个四不像的代码实现。  

**文章首发GitHub [https://github.com/Lobster-King/AppArticles](https://github.com/Lobster-King/AppArticles)**


<a href="https://www.vultr.com/?ref=7766366"><img src="https://www.vultr.com/media/banner_2.png" width="468" height="60"></a>

