[Does deactivating a GameObject automatically stop its coroutines?](https://discussions.unity.com/t/does-deactivating-a-gameobject-automatically-stop-its-coroutines/10107)

Note: Coroutines are not stopped when a MonoBehaviour is disabled, but only when it is definitely destroyed. \
You can stop a Coroutine using MonoBehaviour.StopCoroutine and MonoBehaviour.StopAllCoroutines. \
Coroutines are also stopped when the MonoBehaviour is destroyed.

师父曾经说过所有Unity里的UI组件都是monobehaviour。\
给UI写了个Coroutines，让它出现的时候开始跑，消失的时候停。\
写在了Awake/Start里StartCoroutines。

有问题。得写在OnEnable()，StartCoroutines(Scrolling)。\
OnDisable()，StopCoroutines(Scrolling)。

[Coroutines](https://docs.unity3d.com/2017.4/Documentation/Manual/Coroutines.html)
This effectively means that any action taking place in a function must happen within a single frame update; \
a function call can’t be used to contain a procedural animation or a sequence of events over time.

A coroutine is like a function that has the ability to pause execution and return control to Unity but then to continue where it left off on the following frame.
```cs
IEnumerator Fade() {
    for (float f = 1f; f >= 0; f -= 0.1f) {
        Color c = renderer.material.color;
        c.a = f;
        renderer.material.color = c;
        yield return null;
    }
}
```
所以说是每一帧跑到yield return null会等，然后下一帧继续跑刚刚的loop，的意思吧。

In UnityScript, things are slightly simpler. \
Any function that includes the yield statement is understood to be a coroutine and the IEnumerator return type need not be explicitly declared:
>刚刚的函数写成function Fade()也行的。\
>使用时不同StartCoroutines也行的(?!
