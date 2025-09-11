先聊会天吧。\
如果要做一个任务，到了现场发现 要啥啥工具没有，打年糕从种小麦开始。\
应该用多少时间去搞工具？多少时间去做真正的年糕？

>https://docs.unity.cn/cn/2021.3/Manual/class-PlayerSettingsiOS.html

想想这两天打包都是打些啥啊……\
噢，先是拉了更新，发现PlayFab library里面有个class读不到，使用了那个class（PlayFabServerAPI）的method。\
花了很长时间去看那个class缺失的method，PlayFab是否忘导入代码，把PlayFab相关的SDK删了重装了。\
然后比对老代码，老代码就是那样写的，就是用了那个class和method没有任何问题。\
（中途还有公司打包用电脑IDE全炸，IDE过期，苹果的直接乱码，windows全红读不出来，震撼，这个小看一下放弃搞了）。

终于想到去看读不出来的class的代码（因为IDE的问题不是很方便看而搁置），发现噢是有个两个常数在辨别，unity内代码是灰色的。
```
#if ENABLE_PLAYFABSERVER_API && !DISABLE_PLAYFAB_STATIC_API
```
没有满足条件而没有被编辑啊。\
然后看这两个常数在哪里设置。\
查了半天才发现不是代码里，不是PlayFab上，是一个叫做scripting define symbols的东西。\
在player里面，如果define了这些，那代码可以编译。\
到这里花了很长时间把更新跑起来了。太好了。

然后是真正打包的地方。\
一般是在external dependency manager内装cocoapods的。\
Cocoapods出问题了。
>https://blog.csdn.net/iningwei/article/details/136713925
>https://blog.csdn.net/ylgwhyh/article/details/50542688
>https://www.jianshu.com/p/7066b5756a35
>https://www.jianshu.com/p/f43b5964f582

按这个改了很久，先是ruby太老了，装上后cocoapods还有问题。\
然后改了很久ruby和cocoapods，把最旧的ruby删掉了（可能被默认用了？）。重启了unity，好了。\
重启unity也是必要条件之一可能。

报Xcode里有些值找不到了，重新生成xCode相关的文件又没事了。\
然后就是……Brust compiler出问题了。
```
##llvm-lipo: error: 'Temp/StagingArea/StaticLibraries/lib_burst_generated64.a': 'Temp/StagingArea/StaticLibraries/lib_burst_generated64.a': Unknown attribute kind (86) (Producer: 'APPLE_1_1700.0.13.3_0' Reader: 'LLVM 13.0.0')
```
花了很长时间才发现是brust的问题，brust用的是LLVM，brust是unity/苹果自带的。
>https://discussions.unity.com/t/buildfailedexception-burst-compiler-failed-running/913610/5

这个哥直接把人关掉了，行吧，暂时没事了。\
顺便一提ai讲可以1. 更新 Unity Burst 包，2. 升级/降级 Xcode，3. 禁用 Burst 编译（临时解决方案），4. 清理项目
```
# 删除以下目录：
rm -rf Library/
rm -rf Temp/
rm -rf obj/
```
5. 更新 Unity，8. 重新导入所有 Burst 相关文件\
看包怎么样吧。

provisioning profile报错了，这个找运营下礼拜再说吧。
>https://stackoverflow.com/questions/3362652/what-is-a-provisioning-profile-used-for-when-developing-iphone-applications

还有个找运营的事情，说是GoogleService-Info.plist没有，这个在firebase上生成，然后放在assets文件目录下。
>https://zhuanlan.zhihu.com/p/548947889
