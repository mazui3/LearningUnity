Fire-and-forget is useful in situations where you don’t need the result of an operation immediately, and it can happen in the background.

>https://www.enterpriseintegrationpatterns.com/patterns/conversation/FireAndForget.html
>https://techcommunity.microsoft.com/blog/educatordeveloperblog/fire-and-forget-methods-in-c-%E2%80%94-best-practices--pitfalls/4299605

Microsoft里面的解释讲的挺好的…大概是fire一个await task但是不需要追踪，让它自己发送完就行。\
有点像coroutine但是不需要await？

在一forget是班码里有这么一段自我倒计时.\
param应该字如其意，用token叫停就会停。
```cs
protected override void OnOpen()
{
  Refresh();
  cts = new CancellationTokenSource();
  CountDown(cts.Token).Forget();
}

private async UniTaskVoid CountDown(CancellationToken token)
{
  var serverTime = ServerTimeManager.Instance.ServerTime;
  var resetTime = tomorrow - serverTime;
  View.TMPRefreshTime.SetText(Global.GetFormatTime(tomorrow));
  await UniTask.WaitForSeconds(1, cancellationToken: token);
  if (token.IsCancellationRequested)
      return;
  tomorrow = serverTime.Date.AddDays(1);
  if (resetTime.TotalSeconds == 0)
  {
      Refresh();
  }
  
  CountDown(token).Forget();
}
```
