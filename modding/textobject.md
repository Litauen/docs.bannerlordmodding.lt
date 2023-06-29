# TextObject

[Community Documentation](https://docs.bannerlordmodding.com/_csharp-api/localization/textobject.html){target=_blank}


## SetTextVariable

``` cs
text = new TextObject("{=ZGkcnvDN}I am grateful, {HERO}. I can tell you my family is currently seeking out proposals.", null).SetTextVariable("HERO", Hero.MainHero.Name);
```

## Global text variables

``` cs
MBTextManager.SetTextVariable("REFUSE_RESPONSE", "Do I look like I know anything about trading?", false);
```
