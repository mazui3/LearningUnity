在偷偷看隔壁组的项目。

隔壁组做休闲点点乐……可以自动化胡闹厨房gameplay类游戏吧。\
胡闹厨房是手柄操作移动，隔壁组是点击目的地排序，操控一个角色行动。

角色的行动隔壁组用了**A* Pathfinding Project**这个插件。
>https://arongranberg.com/astar/documentation/stable/index.html

略读代码。

给行动角色挂上了Seeker，AILerp的script。
>Lerp stands for linear interpolation.
>To Lerp means to move from point A to point B by an amount t, where t is greater than or equal to zero and less than or equal to one.

在地图上打上A*用的点（nodes）。\
父级挂AstarPath，子级是各个在地图位置上不同的Node，每个node挂了NodeLink。
子和父之间还留了一层，挂了叫UnityReferenceHelper的代码
>Helper class to keep track of references to GameObjects.
>Does nothing more than to hold a GUID value.

这个结构应该是插件提供的。
