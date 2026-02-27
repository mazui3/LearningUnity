PlayerPrefs is a class that stores Player preferences between game sessions. It can store string, float and integer values into the user's platform registry.

Unity stores PlayerPrefs in a local registry(主程哥也把playerPrefs叫做注册表), without encryption. Don't use PlayerPrefs data to store sensitive data.

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

if (PlayerPrefs.HasKey("currentXP"))
{
    currentXP = PlayerPrefs.GetInt("currentXP");
}
```

_注册表和存档的区别是什么_
>注册表是Windows系统用于存储硬件、软件配置和用户设置的中央数据库，实时影响系统运行；而存档（Archive）是指将文件、数据归档保存，侧重于历史数据备份或持久化存储，不直接参与系统当前运行。注册表是“配置数据”，存档是“数据文件”。
>注册表数据主要存在 C:\Windows\System32\config 系统目录下，用于记录软件配置、键值，而游戏存档、文档等数据文件通常分散在用户文件夹（如 AppData、Documents）中。
