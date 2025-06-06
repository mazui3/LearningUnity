检测whether child object being masked by parent object.

有个很牛逼的canvasRender.cull判定
https://docs.unity3d.com/6000.1/Documentation/ScriptReference/CanvasRenderer-cull.html

bool IsChildVisible(Graphic childGraphic) {
    // 检查子物体是否被Mask裁剪（仅适用于直接子物体）
    return childGraphic.canvasRenderer.cull == false;
}

豆包：此方法仅能判断子物体是否被直接父级 Mask 裁剪，无法检测多层嵌套 Mask 的情况。
但这个得直接判定物件的canvasRender(image之类的UI都有自带canvasRender),在start后生成在嵌套类物体后的,不好拿它的children.

用了笨办法,判定child and parent's rect transform, whether overlay or not.
RectTransform -> GetWorldCorners -> convert to Rect -> compare.
