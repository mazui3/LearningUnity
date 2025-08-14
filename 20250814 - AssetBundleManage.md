Tools -> BuildPieceAssetBundle -> Android\
如果使用assetBundle的话，打包前记得得把所有更改过的预制体打进AB包。\

```
MeshRenderer sr = obj.GetComponent<MeshRenderer>();
if (null != sr)
{
  sr.sharedMaterial.shader = Shader.Find(sr.sharedMaterial.shader.name);
}
```
在代码中看有特殊处理shader？

看到有行注释说“Spine加载材质可能延迟，所有我们需要在这种情况下等待下在去赋值shader”\
```
pl.transform.DOScale(pl.transform.localScale, 0.1f).OnComplete(() => 
  {
      if (sr[k].sharedMaterial==null)
      {
          sr[k].sharedMaterial = new Material("skeleton_Material");
          sr[k].sharedMaterial.shader = Shader.Find("Spine/Skeleton");
      }          
});
```
还用dotween做了，牛逼。
