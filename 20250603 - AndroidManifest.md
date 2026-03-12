[AndroidManifest.xml](https://blog.csdn.net/qq_35430000/article/details/111205079)


 `AndroidManifest.xml` 就像是这款游戏的**“户口本”**或者**“说明书”**。

---

### 核心层级拆解

我们可以把这个 XML 看作一个**剥洋葱**的过程，从外到内依次是：

#### 1. `<manifest>` (最外层：身份证)

这是文件的根节点。

* **package**: 你的包名（如 `com.joynow.game`）。这是 App 在世界上唯一的 ID。
* **versionCode / versionName**: 给系统看的版本号和给用户看的版本名。

#### 2. `<uses-permission>` (申请权限)

写在 `application` 标签之外。

在安卓系统里，出于安全考虑，App 不能随便动手机的功能（比如联网、存文件、收钱）。如果你想用这些功能，就必须在 AndroidManifest.xml 里提前向系统“打报告”。

💡 小贴士：普通权限 vs 危险权限\
 - 普通权限（如 INTERNET）：只要你在 Manifest 里写了，安装时系统会自动允许，不需要弹窗问用户。
 - 危险权限（如 摄像头、定位）：即使你写在 Manifest 里了，游戏运行时还是会弹出一个窗口问玩家：“你允许这个 App 使用摄像头吗？”（这就是所谓的 运行时权限）。

#### 3. `<application>` (核心配置)

这是最重头戏的部分。\
虽然你的游戏可能有很多功能（比如接入了支付、广告、各种 SDK），但对于手机系统来说，这些功能都属于同一个 App 实体。\
所以，所有的 Activity、Service（后台服务）、权限声明等，都必须统统塞进这唯一的一个 <application> 框框里。

* **android:icon / android:label**: 决定了桌面上显示的**图标**和**名字**。
* **android:theme**: 决定了 App 的基础皮肤（比如有没有标题栏，是不是全屏）。

#### 4. `<activity>` (具体的界面/房间)

Android 的每一个界面（甚至是没界面的逻辑脚本）都叫 Activity。

* **android:name**: 指向具体的代码类（比如 `com.unity3d.player.UnityPlayerActivity`）。
* **android:exported**: 是否允许“外人”启动这个界面。如果是启动页，通常必须为 `true`。

#### 5. `<meta-data>` 是一个 “万能挂件”

在 AndroidManifest.xml 中，当标准字段（比如图标、名字、权限）不够用时，开发者可以用 meta-data 标签来存放一些自定义的、额外的信息。

通常这些信息是给 第三方 SDK（如广告、支付、数据统计）或者 系统底层 读的。

它通常由一个“键值对”组成：

```xml
<meta-data android:name="这是名字" android:value="这是具体的内容" />
```

比如你接入了 Google AdMob 广告或者某个支付 SDK，它们会要求你在 Manifest 里塞入一个 App ID。

* **例子：**
```xml
<meta-data android:name="com.google.android.gms.ads.APPLICATION_ID" android:value="ca-app-pub-xxxxxxxx~xxxxxxxx"/>

```

*系统运行到广告代码时，会去 Manifest 里翻找这个 `meta-data`，确认你是哪个开发者。*

Unity 也会用它来定义一些底层行为。

* **例子：**
```xml
<meta-data android:name="unityplayer.SkipPermissionsDialog" android:value="true" />

```

*这行代码告诉 Unity：启动时先不要自动弹出那个权限申请框，我想在代码里自己控制。*

#### 6. `<queries>` 

软件包查询\
Android 11 之后，如果你要跳转到微信支付或 RuStore，必须在这里声明对方的包名。

---

### 关键过滤器：`<intent-filter>` - 在Activity里面

定义了这个 Activity **能干什么**。

如果你在 Activity 标签里看到下面这对“黄金搭档”，就意味着**“创建一个桌面快捷方式”**：

```xml
<intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
</intent-filter>

```

### 常用字段备忘表

| 字段名 | 简单理解 | 避坑指南 |
| --- | --- | --- |
| **android:screenOrientation** | 屏幕方向 | Unity 游戏通常固定为 `landscape` (横屏) 或 `portrait` (竖屏)。 |
| **android:configChanges** | 状态改变监听 | 比如屏幕旋转或接上键盘。设置了这个，系统就不会在旋转屏幕时强行重启你的 Activity（这对 Unity 性能很重要）。 |
| **android:hardwareAccelerated** | 硬件加速 | 通常设为 `true`，让游戏跑得更顺滑。 |

---
