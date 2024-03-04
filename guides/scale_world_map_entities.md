# Scale World Map Entities

Huge thanks to **Iberian Wolf** for doing majority of the research where info on how to scale the entities is stored and **order_without_power** for helping me to solve ranged siege engines scaling in the code.

My goal was to scale down World Map entities for the map to feel larger.


## Towns/castles/villages

Towns/castles/villages can be easily scaled in the Editor using the Inspector - Transform tab:

![](/pics/2403031901.png)

or 'y' for scale controls, or holding 'b' when the entity is selected.


## People (characters)

0.3 scale is hard coded for the people in the PartyVisual.AddCharacterToPartyIcon. I used such Harmony Transpiler to change it to 0.15 (half the size):

``` cs
[HarmonyPatch(typeof(PartyVisual), "AddCharacterToPartyIcon")]
public static class ScalePatch
{
    public static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions)
    {
        var newInstructions = new List<CodeInstruction>(instructions);
        for (int i = 0; i < newInstructions.Count; i++)
        {
            if (newInstructions[i].opcode == OpCodes.Callvirt && newInstructions[i].operand is MethodInfo method && method.Name == "Scale")
            {
                if (newInstructions[i - 1].opcode == OpCodes.Ldc_R4 && (float)newInstructions[i - 1].operand == 0.3f)
                {
                    newInstructions[i - 1].operand = 0.15f;
                    break;
                }
            }
        }
        return newInstructions;
    }
}
```

??? example "Result applying only this patch:"
    ![](/pics/2403031912.png)


## Mounts

0.3 hardcoded into PartyVisual.AddCharacterToPartyIcon. Transpiler to change it to 0.15:

``` cs
[HarmonyPatch(typeof(PartyVisual), "AddCharacterToPartyIcon")]
public static class MountScalePatch
{
    public static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions)
    {
        var newInstructions = new List<CodeInstruction>(instructions);
        for (int i = 0; i < newInstructions.Count; i++)
        {
            if (newInstructions[i].opcode == OpCodes.Mul && newInstructions[i - 1].opcode == OpCodes.Ldc_R4)
            {
                if ((float)newInstructions[i - 1].operand == 0.3f)
                {
                    newInstructions[i - 1].operand = 0.15f;
                    break;
                }
            }
        }
        return newInstructions;
    }
}
```

??? example "Result applying only this patch:"
    ![](/pics/2403031920.png)



## Caravan animals

In PartyVisual.AddMountToPartyIcon changing 0.3 -> 0.15

``` cs
[HarmonyPatch(typeof(PartyVisual), "AddMountToPartyIcon")]
public static class ScaleMountPatch
{
    public static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> instructions)
    {
        var newInstructions = new List<CodeInstruction>(instructions);
        for (int i = 0; i < newInstructions.Count; i++)
        {
            if (newInstructions[i].opcode == OpCodes.Mul && newInstructions[i - 1].opcode == OpCodes.Ldc_R4)
            {
                if ((float)newInstructions[i - 1].operand == 0.3f)
                {
                    newInstructions[i - 1].operand = 0.15f;
                    break; // Break out of the loop once we've made our change
                }
            }
        }
        return newInstructions;
    }
}
```


## Ranged Siege Engines

I have tried to scale siege engines in the Blender and reimport the meshes. That way siege engines are scaled, but their animations are not.

??? example "How it looks:"
    ![](/pics/trebuchet_0.5b.gif)

    !!! quote "Swan [ADOD/IAF]:"
        This is an animation issue, although you scaled the object and skeleton, the animation is still keyframed to a set radius.

This patch scales down only ranged siege engines (ballistas, mangonels, trebuchets) with their animations intact:

```cs
[HarmonyPatch(typeof(PartyVisual))]
[HarmonyPatch("AddSiegeMachine")]
public class PartyVisual_AddSiegeMachine_Patch
{
    public static bool Prefix(SiegeEngineType type, MatrixFrame globalFrame, BattleSideEnum side, int wallLevel, int slotIndex, PartyVisual __instance)
    {
        // do not apply this patch for not-ranged siege engines to avoid the crash NullReferenceException in PartyVisual.TickSettlementVisual
        if (!type.IsRanged) return true;

        string siegeEngineMapPrefabName = Campaign.Current.Models.SiegeEventModel.GetSiegeEngineMapPrefabName(type, wallLevel, side);

        // Use reflection to get the PropertyInfo object for the 'MapScene' property
        PropertyInfo mapScenePropInfo = __instance.GetType().GetProperty("MapScene", BindingFlags.NonPublic | BindingFlags.Instance);
        if (mapScenePropInfo == null) return true;
        Scene mapScene = (Scene)mapScenePropInfo.GetValue(__instance);

        GameEntity gameEntity = GameEntity.Instantiate(mapScene, siegeEngineMapPrefabName, true);

        if (gameEntity != null)
        {
            __instance.StrategicEntity.AddChild(gameEntity, false);
            MatrixFrame matrixFrame;
            gameEntity.GetFrame(out matrixFrame);

            // scaling here
            matrixFrame.Scale(matrixFrame.GetScale() * 0.5f);
            gameEntity.SetFrame(ref matrixFrame);

            GameEntity gameEntity2 = gameEntity;
            MatrixFrame matrixFrame2 = globalFrame.TransformToParent(matrixFrame);
            gameEntity2.SetGlobalFrame(matrixFrame2);
            List<GameEntity> list = new List<GameEntity>();
            gameEntity.GetChildrenRecursive(ref list);
            GameEntity gameEntity3 = null;
            if (list.Any((GameEntity entity) => entity.HasTag("siege_machine_mapicon_skeleton")))
            {
                GameEntity gameEntity4 = list.Find((GameEntity entity) => entity.HasTag("siege_machine_mapicon_skeleton"));
                if (gameEntity4.Skeleton != null)
                {
                    gameEntity3 = gameEntity4;

                    // scaling the skeleton
                    gameEntity3.SetFrame(ref matrixFrame);

                    string siegeEngineMapFireAnimationName = Campaign.Current.Models.SiegeEventModel.GetSiegeEngineMapFireAnimationName(type, side);
                    gameEntity3.Skeleton.SetAnimationAtChannel(siegeEngineMapFireAnimationName, 0, 1f, 0f, 1f);
                }
            }

            // Access the private readonly field '_siegeRangedMachineEntities'
            var fieldInfo = AccessTools.Field(__instance.GetType(), "_siegeRangedMachineEntities");
            var siegeRangedMachineEntities = (List<ValueTuple<GameEntity, BattleSideEnum, int, MatrixFrame, GameEntity>>)fieldInfo.GetValue(__instance);


            if (type.IsRanged)
            {
                siegeRangedMachineEntities.Add(ValueTuple.Create<GameEntity, BattleSideEnum, int, MatrixFrame, GameEntity>(gameEntity, side, slotIndex, globalFrame, gameEntity3));
                return false;
            }
            siegeRangedMachineEntities.Add(ValueTuple.Create<GameEntity, BattleSideEnum, int, MatrixFrame, GameEntity>(gameEntity, side, slotIndex, globalFrame, gameEntity3));
        }
        return false;
    }
}
```

Sadly this patch crashes when applied to the not-ranged siege engines.

## Not-ranged Siege Engines and Projectiles

To scale down not-ranged siege engines I scaled them down in the Blender and reimported them into the Editor.

!!! warning "I think it's possible to scale them down the same way as ranged siege engines, but I spent too much time till this moment on this problem and don't want to look for a better method. If somebody will fix my previous patch to not crash for not-ranged siege engines - please let me know. I will update this guide.<br><br>Also, I think Transpiler would be better in this case, but again - maybe in the future."

This is how imported and scaled-down entities look in the Resource Browser:

![](/pics/2403031935.png)

??? example "It is necessary to scale down projectiles also, without it it looks like this:"
    ![](/pics/2403031937.png)

