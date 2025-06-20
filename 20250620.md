Text首行出现标点解决方案
>https://blog.csdn.net/qq_42672770/article/details/132063879
>
>https://www.cnblogs.com/nianxiaoyu/p/18633109


主要是依靠
_OnPopulateMesh(VertextHelper vh)_ 这个函数
>https://blog.csdn.net/qq_42672770/article/details/132063879
每当Text内的内容改变，(会重新生成顶点或纹理？）然后这个function就会再被call一遍。
所以它本身可以当作一种recursively function。

还有判别是否数字在同一行，使用：

```
private System.Text.StringBuilder MExplainText = null;

private IList<UILineInfo> MExpalinTextLine;

MExpalinTextLine = _component.cachedTextGenerator.lines;
```

这个UILineInfo中自带一个startCharIdx，这一行的第一个char在整个text（Text class中自带text variable）string中的位置。
基本用这个startCharIdx来辨别首行和尾行的char（没有直接的尾行！得计算！）


回顾用regex比较
>https://stackoverflow.com/questions/22937618/reference-what-does-this-regex-mean

因为有两个需求，标点和数字，一开始写了两个Coroutine，发现，会打架。
（可能处理得好不会？但如果句号在末尾，直接那上一个字来做切割可能会切到数字，得一起思考吧。）

```
//数字间隔断
if (_lastCharIsNum == true && _firstCharIsNum == true)
{
    //寻找上一个可以截断的index
    int j = 0;

    //记得心算index啊啊啊啊
    while(Regex.IsMatch(_component.text[MExpalinTextLine[i].startCharIdx - j - 1].ToString(),  numRegex) == true)
    {
        j++;
    }
    mChangeIndex = MExpalinTextLine[i].startCharIdx - j;
    MExplainText.Insert(mChangeIndex, "\n");
}

```
