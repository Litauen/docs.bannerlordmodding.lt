# Battle Scene Grid

* [Battle Scenes List](https://docs.google.com/spreadsheets/d/1mpRIgj6pH34Lkpe30AVHdyEw_8uQ7liPqmRbwYqQlec/edit?gid=0#gid=0)
* [Battle Scenes List](https://docs.google.com/spreadsheets/d/1gyDYwDKxxefs2y3v6KNGbag9jQCxtG0BQg7f_n78z1E/edit?gid=0#gid=0) by Fatrod

<br><br>

`worldmap_battle_scene_grid` image together with `\ModuleData\sp_battle_scenes.xml` define which battle scene will be used in the battle based on the position of the battle on the world map.

## Scene selection

The game takes position point's RED value (0..255) and based on this value selects the scene.

![](/pics/2411201517.png)

Scenes are defined in the `\SandBox\ModuleData\sp_battle_scenes.xml`

Like this:

```xml
<Scene
    id="battle_terrain_020"
    terrain="Plain"
    forest_density="Low"
    map_indices="86, 35, 121, 122, 59, 55, 56, 57, 63, 110, 117, 144">
    <TerrainTypes>
        <TerrainType
            name="Water" />
        <TerrainType
            name="Mountain" />
    </TerrainTypes>
</Scene>
```

`map_indices` define the point's RED value.

In this example it means, that if position's point RED value is one of those: 86, 35, 121, 122, 59, 55, 56, 57, 63, 110, 117, 144 - then scene `battle_terrain_020` can be used in the battle.

Many scenes can have same map_indices and game selects the scene by such algorithm:

![](/pics/2411201532.png)


## Orientation

GREEN channel should define the orientation how parties enter the battle map - from which side.

??? info "Some info about it [here](https://discord.com/channels/411286129317249035/761302555308720148/932014023199850586)"
    ![](/pics/2411201540.png)



## sp_battle_scenes.xml

Default file (in \SandBox\ModuleData) is a mess, looks like this:

```xml
<Scene
    id="battle_terrain_020"
    terrain="Plain"
    forest_density="Low"
    map_indices="86, 35, 121, 122, 59, 55, 56, 57, 63, 110, 117, 144">
    <TerrainTypes>
        <TerrainType
            name="Water" />
        <TerrainType
            name="Mountain" />
    </TerrainTypes>
</Scene>
```

Looking at the code it seems that only `id` and `map_indices` are used, so all this can be simplified to:

```xml
<Scene id="battle_terrain_020" map_indices="86, 35, 121, 122, 59, 55, 56, 57, 63, 110, 117, 144" />
```


### Custom sp_battle_scenes.xml

By default game loads every `ModuleData/sp_battle_scenes.xml` in every mod. Usually that's not what we want because that would create a mess.

It should be possible to overwrite native sp_battle_scenes.xlm with XSLT, but that leaves the possibility that scenes will be loaded from other mods.

This patch loads ONLY our own scenes and eliminates all possible problems:

```cs
[HarmonyPatch(typeof(Campaign), "InitializeScenes")]
public class Campaign_InitializeScenes_Patch
{
    public static bool Prefix()
    {
        GameSceneDataManager.Instance.LoadSPBattleScenes(ModuleHelper.GetModuleFullPath("YOUR_MOD_NAME") + "ModuleData/sp_battle_scenes.xml");
        GameSceneDataManager.Instance.LoadConversationScenes(ModuleHelper.GetModuleFullPath("Sandbox") + "ModuleData/conversation_scenes.xml");
        GameSceneDataManager.Instance.LoadMeetingScenes(ModuleHelper.GetModuleFullPath("Sandbox") + "ModuleData/meeting_scenes.xml");
        return false;
    }
}
```


## worldmap_battle_scene_grid

Native's size is 1024x1024, not sure if other sizes would work.

!!! danger "IMPORTANT! Upload it into your mod's Assets folder in Editor and check these settings:"
    ![](/pics/2411201715.png)

??? abstract "Examples:"
    ![](/pics/2411201736.png)

