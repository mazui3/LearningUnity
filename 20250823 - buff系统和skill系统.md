他们互相循环了…

call **SkillManager.handleSkillData**\
其中有一个技能SkillA会增加一个buff。\
增加完buff后直接call **BuffLogifc.HandleBuff**\
很有道理啊。\
问题是HandleBuff里已经有一个BuffB（其他回合用其他技能B加上的，技能B本身没问题）。\
BuffB是更改属性，更改属性后会call **SkillManager.handleSkillData**。\
然后循环了。

想到的处理方法是有这么一个function，**SkillManager.handlePiecesTargetSkill(pl, buffData.skillID)**\
只处理对应的skill，而不是所有的skill。就是buffB不该call到skillA。

看一下为什么BuffB得用上skillManager.handleSkillData。\
BuffB给pieceLogic增添了dynamicFunction，然后用skillManager处理dynamicFunction了吧。\
dynamicFunction像是给pieceLogic附加的属性。\
如果pl拥有某一个dynamicFunctionC，那在处理所有属性的时候，跑到C时处理一下，这样。
