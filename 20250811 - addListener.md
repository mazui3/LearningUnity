其实不是很理解…我们复习一遍。
在EventsDefine了一个event，要监听的内容。

在需要监听的system里面加入。
```
protected override void OnInit()
{
    this.GetModel<theModel>().InitDoubleChance();//这行代码是啥不知道
    TypeEventSystem.Global.Register<OnOurMethodEvent>(OnOurMethod);
}
```
这个地方OnOurMethodEvent是在EventsDefine的集合体中定义的。
OnOurMethod是写在其监听system里面，如果监听到了这个Event就跑OnOurMethod。
```
protected override void OnDeinit()
{
    base.OnDeinit();
    TypeEventSystem.Global.UnRegister<OnOurMethodEvent>(OnOurMethod);
}
```
系统结束时记得UnRegistered，不然会报错。

然后是真正发送这个信息的地方，被监听的系统。
```
//普通的其他method处理事情。
//需要告诉监听的system时：
TypeEventSystem.Global.Send(new OnOurMethodEvent()
{
    //  这里传输触发时需要知道的数据
    ItemConfig = this._Config
});
```

在监听的系统里，
```
private void OnOurMethod(OnOurMethodEvent e)
{
    //e.ItemConfig
}
```
进行真正数据的处理。
