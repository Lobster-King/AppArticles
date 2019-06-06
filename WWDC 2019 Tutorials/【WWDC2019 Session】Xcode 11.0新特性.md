### 写在前面
**Apple WWDC 2019** 正如火如荼的进行着。按照WWDC计划，美国时间从Tuesday到Friday都会有各种Session分享（实在干货）。所有Session相关的Video、PPT都将陆续在 **[WWDC2019官网](https://developer.apple.com/wwdc19/schedule/#!/)** 放出。

撸主会根据Session Topic与实际工作的相关性并结合实际操作，陆续将Session内容第一时间分享给大家。**因Beta版本的不稳定以及操作姿势的不同，可能会导致大家实操的结果与文中有出入，所以有问题请大家及时反馈。**

进入正题，第一篇分享的是Xcode 11新特性。  

>**PS：Beta版Xcode 11非常不稳定，经常闪退，各位在实操的时候注意稳定情绪。WTF😠!!!**

### Xcode 11新特性

Xcode 11主要包含如下新特性：

* **Xcode工作流更新。**
* **Swift包管理工具（Xcode终于有了自己的包管理工具了😅）。**
* **Source Control代码管理模块更新。**
* **StoryBoard设计工具。**
* **Debug工具更新。**
* **Testing模块更新。**
* **Instruments性能分析工具。**
* **全新的UI框架SwiftUI。**

下面分别介绍这些新特性。

#### 一、Xcode工作流更新

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/workflow.png)

**先来看下Xcode 10右上导航的样子** 

![](https://github.com/Lobster-King/AppArticles/raw/master/WWDC%202019%20Tutorials/xcode10navi.png)

**再看下Xcode 11导航变化**  

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/xcode11navi.png)

**Library变化**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/xocde11library.png)

**新增Editor Options和Add Editor、以及MiniMap**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/minimap.png)

**其他重要能力提升**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/codecompletion.png)

**主要的变更总结如下：**

* Library（代码块Snippets和图片资源预览），由花括号改成了“+”号。原先Snippets、图片预览两项功能扩展为4项：1.SwiftUI控件库。2.SwiftUI Modifiers。3. Snippets代码块。4.图片资源预览（包括后面提到的SPM中的图片资源）。5.Color选择器。

* Version Editor中把Authors（以作者为视角查看代码更改记录）、Logs（以提交时间为视角查看代码更改记录）移出（集成到Editor Options中），只保留Code Review（Comparison代码比对）功能。

* 新增Editor Options和Add Editor功能区。其中Editor Options包含：展示/隐藏SwiftUI画布、展示/隐藏Authors视角、MiniMap等。Add Editor可以新增多个编辑窗口，终于不需要几个编辑窗口来回切换了。

* 新增MiniMap功能。有了代码地图预览功能，查找代码不再繁琐。搜索代码，MiniMap也不同步高亮显示。鼠标在MiniMap移动时，也会有响应方法高亮提示的功能。

* 其他重要功能提升。主要包括：代码不全能力提升、拖拽代码能力提升、拼写检查等。（时间和篇幅原因这部分功能笔者暂未进行实践）

**从上面的更新Tips来看，Xcode正在往好的方向发展，之前饱受开发者诟病的沙雕功能也已慢慢被Apple优化。MiniMap、和Add Editor是个亮点，有了这两个重要功能加持，跟其他IDE相比起码在易用性上不落下风。**

#### 二、Swift包管理工具（SPM）

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/swiftpackagemanager.png)

Xcode终于有了自己的包管理工具了！！！喜大普奔，笔者赶紧尝试了一下SPM。

**SPM入口**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/spmentrance.png)  

用Xcode 11打开工程，点击Project，在Info、Build Setting后面多了一个Swift Packages选项，这就是SPM入口。

点击+号，Xcode会提示你绑定一个账号： **BitBucket、GitHub、GitLab**

笔者绑定的是GitHub账号，Xcode会按照时间顺序自动检索出该账号下的repositories。

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/repos.png)

**添加一个repository到工程（笔者加入的是Alamofire Swift网络库）**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/thirdlibrary.png)

添加完成之后，会在文件目录的最下方出现**Swift Package Dependencies目录**

**使用新加入的repository**

![](https://raw.githubusercontent.com/Lobster-King/AppArticles/master/WWDC%202019%20Tutorials/libraryusage.png)

**适配现有工程、创建Swift Package库、二进制库**

这部分内容会在其他Session进行分享，敬请期待。


