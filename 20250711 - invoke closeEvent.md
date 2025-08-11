close event真是个好东西。
让东西onDestory的时候invoke一下。
来回back and forward communication(....?)

前两天自己写了一个，写出来后很乐（倒是写更熟练点！）。
它是这样构造的。
MainPanel和SubPanel。
SubPanel是做成了一个UIPanel class，同时自带一个UIPanelData的class。
>这个构造是从QFramework来的！直接用。
>在装了QFramework后，用它的方式来create UI。好像是直接右键UI Kit - Create UICode。

在Sub里implement UIPanelData一个Action CloseCallBack。
然后在closeButton method里面mData.CloseCallBack?.Invoke();

在Main里面这么传它。
theBtn.onClicked.AddListener(() =>
{
  SubPanelData mData = new SubPanelData()
  {
    CloseCallBack = () => {
      //如果要改MainPanel里面的东西的话，比如MainPanel.Find("theBtn").gameObject.SetActive(theCondition);
    }
  }
  UIKit.OpenPanel<SubPanelData>(mData);
});
