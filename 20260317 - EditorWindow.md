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
    w.Init();//自己写的
    w.Show();//自带的
  }
}

```

```cs
public class EventListWindow : EditorWindow
{
  private BoardLogic boardLogic;
  GameEventEx[] ges = null;

  public void Init()
  {
    //读取game events
    boardLogic = mTargetObject.GetComponent<BoardLogic>();
    if (boardLogic.m_GameEvent != null)
    {
        int cnt = boardLogic.m_GameEvent.Length;
        ges = new GameEventEx[cnt];
        for (var i = 0; i < cnt; i++)
        {
            ges[i] = new GameEventEx();
            ges[i] = boardLogic.m_GameEvent[i];
        }
    }
    //ges = mTargetObject.GetComponents<GameEvent>();
    Array.Sort(ges, (eventA, eventB) => eventA.mIdx.CompareTo(eventB.mIdx));
  }

  private Vector2 pscroll;
  private void OnGUI()
  {
    pscroll = EditorGUILayout.BeginScrollView(pscroll);
    //scroll view的内容
    EditorGUILayout.EndScrollView();

    //窗口加窗口  
    if(GUILayout.Button("添加事件"))
    {
        Rect wr = new Rect(200, 200, 300, 400);
        addEventWindow aew = (addEventWindow)EditorWindow.GetWindowWithRect(typeof(addEventWindow), wr, true, "添加事件");
        aew.mTargetObject = mTargetObject;
        aew.listWindow = this;
        aew.Init();
        aew.Show();
    }

    EditorGUILayout.BeginHorizontal();
    //一些内容
    EditorGUILayout.EndHorizontal();
  }
}
```

把内容从编辑器保存到prefab中。
```cs
//serializable -> 应该是手动连上的
public GameObject mTargetObject;

//在AddEventWindow class下的值
string msg = "";

private void OnGUI()
{
  //编辑时读取
  msg = EditorGUILayout.TextArea(msg, GUILayout.Height(50));
}

private void AddEvent()
{
  GameEventEx ge = new GameEventEx();
  ge.m_msgContent = msg;
  //ge assign to mDatas

  //用mTargetObject传的
  BoardLogic bo = mTargetObject.GetComponent<BoardLogic>();
  bo.m_GameEvent = mDatas;
}
```

不过它是Modify和Add写了两个Editor Window。
