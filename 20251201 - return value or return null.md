一提嘴代码里有这种写法.
```cs

public theObject ReturnAObject(int param)
{
  if (param > someLimit)
  {
    return null;
  }
  else
  {
    return new theObject();
  }
}
```
然后用读到theObject是否为null可以判定是否能grab an object.\
挺干净的.
