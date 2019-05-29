## 在已有的iOS工程集成Flutter

我会按照官方的文档进行配置，有坑的地方我会在旁边标注。

### 创建Flutter模块

假设你现在的App的路径是这样的```some/path/MyApp```,进入MyApp同级目录下，并创建Flutter模块。

```
$ cd some/path/
$ flutter create -t module my_flutter
```

这个命令会在MyApp同级目录创建一个my_flutter文件夹（Flutter模块），my_flutter文件夹里面包含了Dart Demo工程和一个ios隐藏文件夹，隐藏文件里面还包含了Cocoapods相关东西和一些脚本工具。  

### 让你的工程依赖Flutter模块

最终的文件结构是这样的。

```
some/path/
  my_flutter/
    lib/main.dart
    .ios/
  MyApp/
    MyApp/
      AppDelegate.h
      AppDelegate.m (or swift)
      :
```

### 通过Cocoapods依赖Flutter模块

1.在Podfile中添加如下两行代码。

```
flutter_application_path = '../my_flutter/'
eval(File.read(File.join(flutter_application_path, '.ios', 'Flutter', 'podhelper.rb')), binding)
```

2.执行```pod install```
当你修改了Flutter插件依赖的时候（```some/path/my_flutter/pubspec.yaml```），你需要在my_flutter执行```flutter packages get```来刷新你所创建模块的插件。然后再重新运行```pod install```。

3.关闭Bitcode。目前Flutter并不支持Bitcode优化。

### 添加脚本编译Dart代码
切换到```Build Phases```tab下，新建一个**New Run Script Phase**。添加如下Shell代码。

```
"$FLUTTER_ROOT/packages/flutter_tools/bin/xcode_backend.sh" build
"$FLUTTER_ROOT/packages/flutter_tools/bin/xcode_backend.sh" embed
```

然后把该脚本拖到**Target Dependencies phase**后面。

这时候就可以编译了。

### 在Xcode工程中可以通过FlutterViewController进入Flutter模块。
还需要额外改造下我们的Xcode工程。

Appdelegate.h更改如下。（我试了下，不继承FlutterAppDelegate也没发现什么问题）

```
#import <UIKit/UIKit.h>
#import <Flutter/Flutter.h>

@interface AppDelegate : FlutterAppDelegate <UIApplicationDelegate>

@property (strong, nonatomic) UIWindow *window;
@property (nonatomic,strong) FlutterEngine *flutterEngine;

@end
```

Appdelegate.m在didFinishedLanuched中添加如下代码。

```
self.flutterEngine = [[FlutterEngine alloc] initWithName:@"io.flutter" project:nil];
[self.flutterEngine runWithEntrypoint:nil];
[GeneratedPluginRegistrant registerWithRegistry:self.flutterEngine];
```
在ViewController中添加Flutter跳转入口。

```
#import <Flutter/Flutter.h>
#import "AppDelegate.h"
#import "ViewController.h"

@implementation ViewController
- (void)viewDidLoad {
    [super viewDidLoad];
    UIButton *button = [UIButton buttonWithType:UIButtonTypeCustom];
    [button addTarget:self
               action:@selector(handleButtonAction)
     forControlEvents:UIControlEventTouchUpInside];
    [button setTitle:@"Press me" forState:UIControlStateNormal];
    [button setBackgroundColor:[UIColor blueColor]];
    button.frame = CGRectMake(80.0, 210.0, 160.0, 40.0);
    [self.view addSubview:button];
}

- (void)handleButtonAction {
    FlutterEngine *flutterEngine = [(AppDelegate *)[[UIApplication sharedApplication] delegate] flutterEngine];
    FlutterViewController *flutterViewController = [[FlutterViewController alloc] initWithEngine:flutterEngine nibName:nil bundle:nil];
    [self presentViewController:flutterViewController animated:false completion:nil];
}
@end
```

### 启动热重载或者调试Dart程序

```
$ cd some/path/my_flutter
$ flutter attach
Waiting for a connection from Flutter on iPhone X...
```

**坑1.0**

当我执行flutter attach的时候，flutter直接挂掉了。

```
flutter attach
Checking for advertised Dart observatories...

Oops; flutter has exited unexpectedly.
Sending crash report to Google.
Crash report sent (report ID: 50ca8df4de508ad3)
```

最终在flutter官方issue中找到了答案。**切换sdk源（flutter channel master）**  

**坑1.1**

切换sdk源的时候，又报如下错误。

```
Failed to retrieve the Dart SDK from: https://storage.googleapis.com/flutter_infra/flutter/8dc3a4cde2075a4f5458fd0eb199627f5124508d/dart-sdk-darwin-x64.zip
If you're located in China, please see this page:
  https://flutter.dev/community/china
```

大概意思是让我更改镜像地址，于是我打开[https://flutter.dev/community/china](https://flutter.dev/community/china)，发现还得翻墙才能打开😠。

```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

更改镜像之后，再执行**flutter channel master**。

**坑1.2**

成功切换flutter到master之后，我发现我自己的Xcode工程Run不起来了。（Run的时候一直卡在Build Phase脚本那里）。

抱着试试看的态度，我把之前在stable源创建的Flutter模块干掉，重新创建了一下。然后再执行pod install。竟然好使了！！！

这时候再执行flutter attach就能愉快的跟模拟器链接起来了。

### 不爽之处

貌似flutter没有提供IDE插件，我现在只能在IntelliJ Idea中修改Dart代码，然后回到终端进行热重载，然后再回到模拟器看效果。  

真希望Flutter能够提供一个Xcode插件，可以像Android studio那样直接在IDE里面开发Dart。