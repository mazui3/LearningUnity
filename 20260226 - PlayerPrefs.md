https://docs.unity3d.com/6000.3/Documentation/ScriptReference/PlayerPrefs.html

PlayerPrefs is a class that stores Player preferences between game sessions. It can store string, float and integer values into the user's platform registry.

Unity stores PlayerPrefs in a local registry, without encryption. Don't use PlayerPrefs data to store sensitive data.

Unity自带的，储存用户数据的class。

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
