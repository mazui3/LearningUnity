```cs
//The Coin Manager
public BindableProperty<int> SouvenirCoinNum = new();
```

```cs
//The Coin Panel
SouvenirCoinActivityManager.Instance.SouvenirCoinNum.Register((newValue) =>
{
  RefreshCorrelatedUI();
}).UnRegisterWhenGameObjectDestroyed(this);
```

这是Q frame里面带的listener，很好用。\
强化一下记忆…
