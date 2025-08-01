ScrollView - withScrollRect
>Viewport - Mask
>>Content - GridLayoutGroup & Content Size Fitter
>>>TheChildObjects

的格式真好用啊。

以ScrollView当作页签，另一侧做物品详情页。
衍生在写一种功能，点击某个物品A，会跳转到其他的物品B页面。
在生成ChildObjects的同时，可以写进btn.OnClick();，自带了页签 -> 详情页的method。
但反过来详情页 -> 页签 的顺序不能这么写。
现在有物品B的id，但是此id跟Content里的排序没有关系。
第一反应是搞个array对照一下。
翻了老代码人的确也是这么写的…
也是哦.jpg

找到了第一个child Object再GetChild去找gameObject。
有了gameObject，拿到它的anchoredPosition.y，用DoTween做一个滑动。
先DOKill.
Content.GetComponent<RectTransform>().DOAnchorPosY(-(selectTrans.GetComponent<RectTransform>().anchoredPosition.y + 30), 0.2f);
