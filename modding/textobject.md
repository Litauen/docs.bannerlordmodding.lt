# TextObject

[Community Documentation](https://docs.bannerlordmodding.com/_csharp-api/localization/textobject.html){target=_blank}


## SetTextVariable

``` cs
TextObject text = new TextObject("I am grateful, {HERO}. I can tell you my family is currently seeking out proposals.", null).SetTextVariable("HERO", Hero.MainHero.Name);
```

## Global text variables

``` cs
MBTextManager.SetTextVariable("REFUSE_RESPONSE", "Do I look like I know anything about trading?", false);
```

## Variations based on the property

```cs
TextObject textObject = new TextObject("You are known as a {?HERO.GENDER}woman{?}man{\\?} of honor. You may know me as one as well.", null);
StringHelpers.SetCharacterProperties("HERO", hero.CharacterObject, textObject, false);
```
