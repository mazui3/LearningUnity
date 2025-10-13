https://docs.unity3d.com/6000.1/Documentation/Manual/gradle-templates.html

今天打包。\
升级了一下facebook的SDK，从17升级18。\
升级完没有改mainTemplate.gradle中的设定。\
mainTemplate.gradle中应该是refernece了加入的SDK，里面写的不是18而是17就报错了。

不过改过之后不知道为什么在Android Resolver Exclusions Start的区域里多了一行字，指向NDKPATH。\
然后报错说NDKPATH有问题。\
去掉了就好了。

10.13记\
我他妈终于搞懂了一件事情.\
mainTemplate.gradle中的格式是这样写的.
```
implementation 'com.adjust.sdk:adjust-android:5.4.4' // Assets/Adjust/Native/Editor/Dependencies.xml
```
_com.adjust.sdk:adjust-android:5.4.4_ 是要加的dependencies class,而后面的那个是插件要的dependencies说明地址.\
在dependencies.xml中已经讲了要添加的内容。
