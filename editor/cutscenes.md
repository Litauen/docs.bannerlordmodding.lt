# Cutscenes


* [Guide](https://docs.google.com/document/d/1iyZaWk9gq3CPm25VWJuCtrtReW65Jzlwk8H8_6lMStM/edit?tab=t.z6s77vm4l37x) by Snorri

Subclass of `SceneNotificationData`:

![](/pics/2508290819.png)


Example how to start/play:

```cs
MBInformationManager.ShowSceneNotification(new MarriageSceneNotificationItem(Hero.MainHero, Hero.MainHero.Spouse, CampaignTime.Now, SceneNotificationData.RelevantContextType.Any));
```

## Change scene for default cutscene

```cs
[HarmonyPatch]
public class NewBornSceneNotificationItem_SceneID_Patch
{
    [HarmonyPostfix]
    [HarmonyPatch(typeof(NewBornSceneNotificationItem), "SceneID", MethodType.Getter)]
    public static void Postfix(NewBornSceneNotificationItem __instance, ref string __result)
    {
        __result = "scn_born_baby_custom";
    }
}
```

!!! warning "Important to know when changing just a scene for a default cutscene"
    * Do not delete characters - they are referenced in code and they should remain in the scene. Move them out of the view if you don't need them.
    * You can freely delete objects (tables, walls, etc) and add new ones - that should not lead to the crash.

## Open palm pose

![](/pics/2508281109.png)

!!! quote "Artem"

Set/force agent to LOD0.

`MBAgentVisuals.SetAgentLodZeroOrMax`

Custom cameras have this issue though. If you spawn the camera too far away from the agents and then zoom in on them their hand/face morphs won't change during the entirety of the cameras duration.