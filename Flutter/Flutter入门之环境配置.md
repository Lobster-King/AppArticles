## MacOS平台安装Flutter环境

### MacOS系统要求

* **操作系统**：macOS（64位）
* **硬盘容量要求**：700MB（不包括IDE）
* **工具集**：Flutter需要安装如下命令行工具  

	bash  
	curl  
	git 2.x  
	mkdir  
	rm  
	unzip  
	which   
	
### 获取Flutter SDK

1.下载最新稳定版SDK [flutter_macos_v1.5.4-hotfix.2-stable](https://storage.googleapis.com/flutter_infra/releases/stable/macos/flutter_macos_v1.5.4-hotfix.2-stable.zip) 。你也可以从这个网址中下载其他版本。[SDK archive](https://flutter.dev/docs/development/tools/sdk/releases)   
2.解压到指定目录  

```
 cd ~/development
 unzip ~/Downloads/flutter_macos_v1.5.4-hotfix.2-stable.zip
```  
3.把flutter加到执行路径中  
```
 export PATH="$PATH:`pwd`/flutter/bin"
```  
注意上面的命令，只是在当前命令行窗口有效，如果你想在每个终端窗口都能够运行flutter命令，则还需要额外的配置。  

* 打开或者创建一个**$HOME/.bash_profile**。加上下面一行
```
 export PATH="$PATH:[PATH_TO_FLUTTER_GIT_DIRECTORY]/flutter/bin"
```  
* 运行**source $HOME/.bash_profile**刷新窗口。
* 通过输入**echo $PATH**命令来校验设置是否正确。

>可以通过**source ~/.bash_profile**命令来检查环境变量设置是否正确。  
>unexpected EOF while looking for matching `"'报错是因为我在变量路径少了一个双引号。  
>正确的设置应该是这样的：**export PATH="$PATH:~/Documents/flutter/bin"**

## 通过doctor命令来检查flutter环境配置

```
 flutter doctor
```

下面是我运行doctor命令，提示我需要配置的地方。（后面配置真机的时候会介绍到）  

```
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, v1.5.4-hotfix.2, on Mac OS X 10.14.3 18D42,
    locale zh-Hans-CN)
[✗] Android toolchain - develop for Android devices
    ✗ Unable to locate Android SDK.
      Install Android Studio from:
      https://developer.android.com/studio/index.html
      On first launch it will assist you in installing the Android SDK
      components.
      (or visit https://flutter.dev/setup/#android-setup for detailed
      instructions).
      If the Android SDK has been installed to a custom location, set
      ANDROID_HOME to that location.
      You may also want to add it to your PATH environment variable.

[!] iOS toolchain - develop for iOS devices (Xcode 10.2.1)
    ✗ libimobiledevice and ideviceinstaller are not installed. To install
      with Brew, run:
        brew update
        brew install --HEAD usbmuxd
        brew link usbmuxd
        brew install --HEAD libimobiledevice
        brew install ideviceinstaller
    ✗ ios-deploy not installed. To install:
        brew install ios-deploy
    ✗ Brew can be used to install tools for iOS device development.
      Download brew at https://brew.sh/.
[!] Android Studio (not installed)
[✓] Connected device (1 available)

! Doctor found issues in 3 categories.
```

## iOS平台配置
### 安装Xcode
1.Xcode版本需要>=9.0
2.如果安装了多个Xcode，需要指定其中一个Xcode。  

```
 sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

3.同意Xcode使用许可。  

```
sudo xcodebuild -license
键盘敲入agree即可（不进行许可是否有问题有待于考证，笔者还没有实验）
```

### 配置iOS模拟器
1.通过以下命令来找到并打开模拟器。

```
 open -a Simulator
```
2.模拟器也需要是支持64位（iPhone 5s及新机型）。  
3.可以通过Window > Scale来更改模拟器尺寸。  

### 创建第一个示例工程

1.通过create命令来创建flutter工程。  

```
 flutter create my_app
```

执行成功后，大致会有这样的log。  

```
All done!
[✓] Flutter is fully installed. (Channel stable, v1.5.4-hotfix.2, on Mac OS X 10.14.3
    18D42, locale zh-Hans-CN)
[✗] Android toolchain - develop for Android devices is not installed.
[!] iOS toolchain - develop for iOS devices is partially installed; more components are
    available. (Xcode 10.2.1)
[!] Android Studio is not available. (not installed)
[✓] Connected device is fully installed. (1 available)

Run "flutter doctor" for information about installing additional components.
```
2.my_app目录创建之后会包含Flutter工程。  
3.通过run命令来启动工程（确保模拟器是打开状态）。  

### 部署到真机设备
要部署到真机设备上，需要安装一些依赖工具，还有一个苹果开发者账号。  
1.安装Homebrew。  
2.确保Homebrew是最新版本。  

```
brew update
```

3.安装以下命令行工具。

```
brew install --HEAD usbmuxd 
/*
执行这句可能报权限错误，Error: The following directories are not writable by your user:
/usr/local/share/man/man8
加入这句即可解决sudo chown -R `whoami`:admin /usr/local/share
*/

brew link usbmuxd
brew install --HEAD libimobiledevice
brew install ideviceinstaller ios-deploy cocoapods
pod setup
```
4.配置签名信息。

* 打开创建的示例工程my_app，打开ios/Runner.xcworkspace。
* 在Xcode中选择Runner target。
* 选择签名证书。

## IDE开发工具

* Android Studio
* IntelliJ IDEA（强烈推荐，感觉是最好用的IDE，没有之一）

1.下载IntelliJ IDEA。[https://www.jetbrains.com/idea/](https://www.jetbrains.com/idea/)  
2.注册激活Idea。[http://idea.lanyus.com/](http://idea.lanyus.com/)  
3.安装Flutter插件。
