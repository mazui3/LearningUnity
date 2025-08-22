上回讲到班中的代码是回合制战斗。\
战斗会分PEvent，例如
 - 单位准备行动
 - 单位移动结束

也有
- 召唤单位事件
- 使用主动技能事件

等等，但给策划的表多了个enum，effectTrigger，触发时机。\
触发时机用PEvent和其他的条件写出来的。

一轮战斗有存档和读档，这个问题比较麻烦。\
没去看为什么这个顺序但先每个回合开始的时候遍历一次单位：\
每一个pieceLogic.newTurnStart();

然后再处理
```
if (!isfirstTrunLoadFromFile)
{
    mBoardData.CurTurnData.m_RoundCount++;
    saveToFile();
}
```

然后反过来再PEvent中的新回合开始时得以
```
Global.isFirstRoundFromFile
```
处理一些特殊情况，比如如果从存档中读取，那第一回合的“己方回合开始时”跑过了，不用再跑了。

问题是为啥，能不能把顺序整理得更清楚一些？

以及之前还遇到过bug，因为同一个触发时间被用于读取数据和数据改变…\
之前的代码应该是想把处理数据和读取数据以技能种类不同来处理，但之后遇到了混乱的情况，手动错新时机分开来来处理了。
