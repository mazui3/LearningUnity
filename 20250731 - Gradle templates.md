https://docs.unity3d.com/6000.1/Documentation/Manual/gradle-templates.html

今天打包呢。
升级了一下facebook的SDK，从17升级18。
升级完没有改mainTemplate.gradle中的设定。
mainTemplate.gradle中应该是refernece了加入的SDK，里面写的不是18而是17就报错了。

不过改过之后不知道为什么在Android Resolver Exclusions Start的区域里多了一行字，指向NDKPATH。
然后报错说NDKPATH有问题。
去掉了就好了。
