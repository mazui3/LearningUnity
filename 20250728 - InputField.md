Unity自带的input field component还能改成密码格式，真不错。

```
InputField.ContentType.Standard;
InputField.ContentType.Password;
```

不过还是老问题，UI没有自动改变，会等到end of frame(?)。
加了一个checkbox的toggle，选择显示密码和隐藏密码。
toggle后更改contentType后手动Transform.textComponent.SetAllDirty();了一下

说起checkBox的toggle。
加上toggle这个component的范围的就是toggle的范围。
所以可以做一个看上去透明的empty gameObject，作为toggle的范围。
显示用的UI/界面就放在子层下。
