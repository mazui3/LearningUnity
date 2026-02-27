PlayerPrefs is a class that stores Player preferences between game sessions. It can store string, float and integer values into the user's platform registry(主程哥也把playerPrefs叫做注册表).

Unity stores PlayerPrefs in a local registry, without encryption. Don't use PlayerPrefs data to store sensitive data.

[Unity自带的，储存用户数据的class。](https://docs.unity3d.com/6000.3/Documentation/ScriptReference/PlayerPrefs.html)

[How to use PlayerPrefs](https://discussions.unity.com/t/how-to-use-playerprefs/183907).

```cs
PlayerPrefs.SetInt("score",5);
PlayerPrefs.SetFloat("volume",0.6f);
PlayerPrefs.SetString("username","John Doe");
PlayerPrefs.Save();

int score = PlayerPrefs.GetInt("score");
float volume = PlayerPrefs.GetFloat("volume");
string player = PlayerPrefs.GetString("username");

bool val = PlayerPrefs.GetInt("PropName") == 1 ? true : false;
```

_注册表和存档的区别是什么_
>注册表是Windows系统用于存储硬件、软件配置和用户设置的中央数据库，实时影响系统运行；\
>而存档（Archive）是指将文件、数据归档保存，侧重于历史数据备份或持久化存储，不直接参与系统当前运行。注册表是“配置数据”，存档是“数据文件”。

>注册表数据主要存在 C:\Windows\System32\config 系统目录下\
>用于记录软件配置、键值，而游戏存档、文档等数据文件通常分散在用户文件夹（如 AppData、Documents）中。

---
想做这么一个事情。

游戏想接俄罗斯的平台，游戏本身使用了playfab作为后台，而俄罗斯无法使用playfab。

运营说想快速上线把游戏跑通，所以存档先全走本地数据，而不走playfab上放的云存档。

班游是商业抽卡游戏，最值钱的就是抽卡。问了主程哥如果玩家物理意义把有资源的存档拷贝，抽不到喜欢的角色再把存档覆盖怎么办。

主程哥说给游戏加时间戳。在注册表里加一个，在存档里加一个，如果两者不匹配，就算有问题。

（主程哥还加了一嘴，存档是非热更新的底层代码，可以挂一个监听事件，真正执行加时间戳的，放在可以热更的地方。）

昨天自己想的是（以为存储信息的地方只有一个）在存档里加时间戳，然后也读取存档这个文件的最后更新时间。如果两者不匹配，就是有问题。

运营说暂时不处理。

……不处理吗。

---

在单机环境下防止“SL大法”（Save & Load）确实是个经典难题。因为在本地环境下，玩家拥有设备最高权限，任何基于本地的校验逻辑，对于硬核玩家或辅助工具来说都是“防君子不防小人”。

### 1. 现有方案的局限性

* **注册表 vs 存档时间戳：** 玩家只需要在备份存档的同时，把对应的注册表项也导出一份（.reg文件），回滚时一起回滚，你的逻辑就失效了。
* **文件最后修改时间：** 这个非常不可靠。操作系统层面的文件修改时间很容易被篡改（比如用 PowerShell 指令），且在某些移动端平台上，覆盖文件的操作可能不会触发预期的修改时间更新。

---

### 2. 更优的单机防作弊策略

如果必须做成全单机，你需要的不是“禁止玩家回滚”，而是让“回滚变得没意义”。

#### A. 伪随机序列提前锁定（最推荐）

不要在玩家点击“抽卡”按钮的那一刻才生成随机数。

* **逻辑：** 在玩家账号初始化时，或者上一次抽卡结束时，就预先生成未来 100 次抽卡的结果序列，并加密存在存档深处。
* **结果：** 玩家无论怎么回滚，他下一次抽出的东西永远是固定的。他回滚 100 次，第 1 次抽出的依然是那张“野猪”，除非他能重置整个账号。

#### B. 增加“回滚成本”：多重校验链

如果你想检测存档是否被替换，可以使用 **哈希链（Hash Chain）**。

* **逻辑：** 存档 A 包含一个 `SessionID` 和 `OperationCount`（操作计数）。每次抽卡，`OperationCount` 加 1。
* **进阶：** 将存档数据与设备的唯一识别码（UID）和特定的本地隐藏文件进行哈希绑定。

#### C. “后悔药”机制（心理博弈）

与其硬碰硬，不如在游戏设计上做文章。

* **设计：** 提供一种代价高昂的“重新抽取”机会，或者加入保底机制。如果玩家发现回滚存档太麻烦（比如要改系统时间、要删隐藏文件），而游戏内有合理的保底，他们往往会放弃作弊。

---

### 3. 针对俄罗斯市场的折中方案

俄罗斯虽然用不了 PlayFab，但并不代表一定要做成“纯单机”。

1. **使用替代云服务：** 俄罗斯当地有非常成熟的互联网基础设施。你可以考虑使用 **VK (VKontakte)** 的游戏 API，或者俄罗斯最大的科技公司 **Yandex** 提供的云服务。他们都有类似 PlayFab 的数据存储 SDK。
2. **自建简单的验证服务器：** 租一个莫斯科或欧洲的节点，只用来存储玩家的核心资产数据（比如抽卡资源和角色列表）。这部分逻辑非常轻量，不需要 PlayFab 那么复杂的整套方案。

---

**抽卡游戏果然还是得联网吗？**
是的。从商业逻辑上讲，**强交互和强资产安全是抽卡游戏的命脉**。如果玩家可以无限刷初始而不付出一丁点代价，你的付费转化率会极低。
