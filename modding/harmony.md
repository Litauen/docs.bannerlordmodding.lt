# Harmony

## Tutorials

* [Harmony documentation](https://harmony.pardeike.net/articles/intro.html){target=_blank}
    * [Transpiler](https://harmony.pardeike.net/articles/patching-transpiler.html)
    * [Another Simple Harmony Transpiler Tutorial](https://gist.github.com/JavidPack/454477b67db8b017cb101371a8c49a1c)
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

## Working with the List

``` cs
[HarmonyPatch(typeof(MapEventSide), "ApplyRenownAndInfluenceChanges")]
public static void Postfix(MBList<MapEventParty> ____battleParties)
{
  foreach(MapEventParty mapEventParty in ___battleParties)
  {
    //...
  }
}
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

## Patching game Models

Usually when game Model is patched, that leads to an error: TypeInitializationException

!!! quote "Explanation from [BannerlordCoop github:](https://github.com/Bannerlord-Coop-Team/BannerlordCoop/blob/development/source/GameInterface/Services/MobileParties/Patches/CalculateBaseSpeedPatch.cs#L34C1-L36C96)"

    /// Fixes issue with DefaultPartySpeedCalculatingModel._culture statically calls GameTexts.FindText<br>
    /// and when harmony patches, it calls the static constructor for DefaultPartySpeedCalculatingModel<br>
    /// and results in a null reference exception because _gameTextManager has not been initialized

It can be solved the same way as BannerlordCoop solved it - initializing _gameTextManager if it's null before Harmony patches. More info [here](https://discord.com/channels/411286129317249035/677511186295685150/1205604280602460161).

Another way is to run Model's patch in the OnGameStart, and the rest of harmony patches in the OnSubModuleLoad.

For that this model's patch should not have [Harmony] tags and it should be executed separately. Example:


``` cs
public class DefaultPartySpeedCalculatingModel_CalculateFinalSpeed_Patch
{
    public static void Postfix(ref ExplainedNumber __result)
    {
        TextObject text = new TextObject("{=}Slow down", null);
        __result.AddFactor(-0.5f, text);
        __result.LimitMin(1f);
    }
}


bool _lateHarmonyPatchApplied = false;

protected override void OnGameStart(Game game, IGameStarter gameStarter)
{
    base.OnGameStart(game, gameStarter);

    if (_lateHarmonyPatchApplied) return; 

    Harmony harmony = new Harmony("my_mod_harmony_late");
    var original = typeof(DefaultPartySpeedCalculatingModel).GetMethod("CalculateFinalSpeed");
    var postfix = typeof(DefaultPartySpeedCalculatingModel_CalculateFinalSpeed_Patch).GetMethod("Postfix");
    if (original != null && postfix != null) {
        harmony.Patch(original, postfix: new HarmonyMethod(postfix));
        _lateHarmonyPatchApplied = true;
    }
}
```

!!! tip "Without _lateHarmonyPatchApplied patch will be applied several times on new game start from menu"

More info about Manual Harmony Patching [here](https://harmony.pardeike.net/articles/basics.html#manual-patching)


## Run before other patch

``` cs
[HarmonyBefore(new string[] { "other.mod's.id" } )]
```

## Unpatch other [patches](https://discord.com/channels/411286129317249035/677511186295685150/1237612303097139273)

``` cs
[HarmonyPatch(typeof(TheType), "TheMethod")]
public static class MyPatches
{
    [HarmonyBefore(new string[] { "other.mod's.id" })]
    static void Prefix(out bool __state)
    {
        if (myCondition)
        {
            HarmonyInstance.Unpatch(AccessTools.Method(typeof(TheType), "TheMethod"), HarmonyPatchType.Prefix, "other.mod's.id");

            __state = true;
        }
    }

    static void Postfix(bool __state)
    {
        if (__state)
        {
            HarmomyInstance.Patch(AccessTools.Method(typeof(TheType), "TheMethod"), HarmonyPatchType.Prefix, "other.mod's.id");
        }
    }
}
```


## Debug

Available via CTRL+ALT+H:

![](/pics/2402281257.png)

