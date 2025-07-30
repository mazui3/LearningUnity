应该是unity自带给ui的drag效果。
在class内带IBeginDragHandler, IDragHandler, IEndDragHandler。

就有

```
public void OnBeginDrag(PointerEventData eventData);
public void OnDrag(PointerEventData eventData);
public void OnEndDrag(PointerEventData eventData);
```

这三个function可以用。
但是不是只能给drag的ui本体加上script，是否能用在parent的script内…
