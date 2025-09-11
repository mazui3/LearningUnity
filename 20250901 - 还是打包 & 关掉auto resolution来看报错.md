今天打安卓，直接卡在build scene0。\
试了1小时没打出来，很怪。下班时让它去打，第二天同事哥说报了16小时还卡着呢。\
我感觉打一个小时大概是危险线了……

姑且升级了一下常见插件，应该不是的，应该没有关系。\
最后用英文搜到了这个贴（还是英文好用啊兄弟！）。
>https://discussions.unity.com/t/unity-2019-2-build-stuck-on-scene-0/228839
>说卡了删library或者设置android resolver。

不过android resolver在现在这个版本不直接在assets文件下了，很怪。\
得去unity-jar-resolver
>https://github.com/googlesamples/unity-jar-resolver

unity-jar-resolver到底是个什么样的存在？感觉应该是官方应该有的东西为啥在github下……

Gradle project files
>https://docs.unity3d.com/6000.2/Documentation/Manual/android-gradle-project-files.html
>https://docs.unity3d.com/6000.2/Documentation/Manual/android-gradle-overview.html
