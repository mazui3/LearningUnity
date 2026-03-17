我累了。

在游戏开发初期，制作游戏关卡编辑窗口也是游戏制作的一部分，还是挺重要的一部分，这么听说。

隔壁组的老策划嘎嘎压力了小主程，讨论过能否减少一个步骤。小主程回了“要是说我现在的能力也只能做到这里呢”，没过多久小主程离职了。

这件事姑且不谈。

EditorWindow是inherited unity这个自带的class制作的，应该。

[一切从这里开始](https://docs.unity3d.com/cn/2021.3/Manual/UIE-HowTo-CreateEditorWindow.html)
```cs
public class WindowMenu
{

  [MenuItem("Window/游戏/添加事件")]
  public static void AddGameEvent()
  {
    GameObject obj = Selection.activeGameObject;
    if (obj == null)
    {
        EditorUtility.DisplayDialog("警告", "请选择游戏棋盘对象", "确定");
        return;
    }
    BoardLogic bl = obj.GetComponent<BoardLogic>();
    if (bl == null)
    {
        EditorUtility.DisplayDialog("警告", "请选择游戏棋盘对象", "确定");
        return;
    }
    Rect wr = new Rect(200,200,800,800);//俺觉得有点小
    EventListWindow w =
        (EventListWindow) UnityEditor.EditorWindow.GetWindowWithRect(typeof(EventListWindow), wr, true,
            "eventList");
    w.mTargetObject = obj;
    w.init();
    w.Show();

  }

}

```
