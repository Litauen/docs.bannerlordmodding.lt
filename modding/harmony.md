# Harmony

## Tutorials

* [Harmony documentation](https://harmony.pardeike.net/articles/intro.html){target=_blank}
* [Reading](https://docs.greenhellmodding.com/client-code-examples/reading-private-variables) / [Modifying](https://docs.greenhellmodding.com/client-code-examples/modifying-private-variables) private variables
* [Changing the Return Value of a Method with Harmony](https://forums.taleworlds.com/index.php?threads/changing-the-return-value-of-a-method-with-harmony.412797/){target=_blank}
* [Lesser Scholar Youtube - Using Harmony. Prefix, Postfix, Transpiler](https://www.youtube.com/watch?v=WmuOjlhulyo)


## Activation

In your SubModule.cs:

``` cs
protected override void OnSubModuleLoad() {
    base.OnSubModuleLoad();
    Harmony harmony = new Harmony("YOUR_MOD_TEXT_ID");
    harmony.PatchAll();
    ...
```


## Changing property with private setter

``` cs
public TextObject Name { get; private set; }

var propertyInfo = AccessTools.Property(typeof(ItemObject), "Name");
var setter = propertyInfo.GetSetMethod(true);
TextObject newItemName = new(item.Name + " ***");
setter.Invoke(item, new object[] { newItemName });
```

## Ambiguous match for HarmonyMethod

``` cs
HarmonyLib.HarmonyException: 'Ambiguous match for HarmonyMethod[(class=TaleWorlds.CampaignSystem.GameComponents.DefaultCharacterDevelopmentModel, methodname=CalculateLearningRate, type=Normal, args=undefined)]'
```

Means that several method exist with the same name but different arguments.<br>
Define arguments for Harmony to properly match the method:

``` cs
[HarmonyPatch(typeof(DefaultCharacterDevelopmentModel))]
[HarmonyPatch("CalculateLearningRate", typeof(int), typeof(int), typeof(int), typeof(int), typeof(TextObject), typeof(bool))]
```

