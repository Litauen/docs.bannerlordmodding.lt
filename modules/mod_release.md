# Mod Release

## Prepare

Change version(s) in SubModule.xml


## Test

Run several years with Better Time mod to make sure there are no crashes.

## Assets

!!! danger "Make whole mod folder backup!"

[Exporting the Module via the Modding Tools](https://moddocs.bannerlord.com/steam-workshop/uploading_updating_mod/#exporting-the-module-via-the-modding-tools){target=_blank} packs Assets/AssetSources/RuntimeDataCache into AssetPackages/pack0.tpac

![](/pics/bFhf0d8.png)

Delete folders:

* Assets
* AssetSources
* RuntimeDataCache
* GUI/SpriteParts
* SceneEditData
<br>


!!! danger "Test in the real game!"



## SubModule.xml

### [DependedModuleMetadatas](https://github.com/BUTR/Bannerlord.BUTRLoader#for-modders){target_blank}

``` cs
  <DependedModuleMetadatas>
    <DependedModuleMetadata id="Native" order="LoadBeforeThis" version="1.1.0"/>
    <DependedModuleMetadata id="SandBoxCore" order="LoadBeforeThis" version="1.1.0"/>
    <DependedModuleMetadata id="Sandbox" order="LoadBeforeThis"  version="1.1.0"/>
    <DependedModuleMetadata id="StoryMode" order="LoadBeforeThis" version="1.1.0"/>
  </DependedModuleMetadatas>
```

## Nexus

### Header image

Header image size: 1536x438

![](/pics/KukQAvR.png)



### Primary image

![](/pics/MN1lY54.png)

For primary image to show properly, aspect ratio is important. Resized image is 236x133, so use these sizes:

    1180 x 665
    1416 x 798
    1652 x 931
    1888 x 1064
    2124 x 1197
    2360 x 1330


## Steam

[Preparing Your Module For Publishing](https://moddocs.bannerlord.com/steam-workshop/uploading_updating_mod/){target=_blank}

!!! abstract "Notes"
    * If you have a custom map, you'd need to patch the path to the settlements_distance_cache.bin in the SettlementPositionScript. It does not format the steam workshop file path correctly.
    * [Example Steam publish scripts/configs](https://github.com/kirnosenko/BannerlordMods/tree/master/publish)

Why some modders don't like Steam:

* They don't earn money from downloads on Steam Workshop
* In order to publish on Steam Workshop it's necessary to launch up the scene editor and do a whole bunch of extra steps
* Auto updates look convenient at first glance but they are forced-fed and can brick hundreds of hours worth of saves easily
* Nightmarish standard folder structure, based on workshop ID number instead of names. It's absolute garbage to backup/maintain a complex list
* You have to pick your image right there; you can't leave it blank and you can't change it later
* Unable to have multiple versions of your mod
