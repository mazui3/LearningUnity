看老代码里面这么做了，一定有其深意吧。
```
Transform eft = pl.transform.Find(buff.Id.ToString());
if (eft != null)
{
    //改个名字再删除，防止影响后续生成
    var eftGO = eft.gameObject;
    eftGO.name += "(Destroy)";
    UnityEngine.Object.Destroy(eftGO);
}
```
