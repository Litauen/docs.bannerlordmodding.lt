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


## Postfix for the getter

``` cs
protected virtual int SkillLevelToAdd
{
    get
    {
        return 10;
    }
}

[HarmonyPatch(typeof(CharacterCreationContentBase), "SkillLevelToAdd", MethodType.Getter)]
protected static void Postfix(int __result) => __result = 30;
```

## Private method match

    [HarmonyPatch(typeof(BarterManager), "ApplyBarterOffer")]

vs when not private:

    [HarmonyPatch(typeof(LeaveKingdomAsClanBarterable), nameof(LeaveKingdomAsClanBarterable.Apply))]


## Ambiguous match for HarmonyMethod

``` cs
HarmonyLib.HarmonyException: 'Ambiguous match for HarmonyMethod[(class=TaleWorlds.CampaignSystem.GameComponents.DefaultCharacterDevelopmentModel, methodname=CalculateLearningRate, type=Normal, args=undefined)]'
```

Means that several methods exist with the same name but different arguments. Method have overrides.<br>
Define arguments for Harmony to properly match the method:

``` cs
[HarmonyPatch(typeof(DefaultCharacterDevelopmentModel))]
[HarmonyPatch("CalculateLearningRate", typeof(int), typeof(int), typeof(int), typeof(int), typeof(TextObject), typeof(bool))]
```

