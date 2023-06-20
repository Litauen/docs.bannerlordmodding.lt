# Mod Release

## Assets

!!! danger "Make whole mod folder backup!"

[Exporting the Module via the Modding Tools](https://moddocs.bannerlord.com/steam-workshop/uploading_updating_mod/#exporting-the-module-via-the-modding-tools){target=_blank} packs Assets/AssetSources/RuntimeDataCache into AssetPackages/pack0.tpac

![](https://imgur.com/bFhf0d8.png)

Delete folders:

* Assets
* AssetSources
* RuntimeDataCache
* GUI/SpriteParts
<br>

```
Test in the real game.
```


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

Header file size: 1536x438


## Steam

[Preparing Your Module For Publishing](https://moddocs.bannerlord.com/steam-workshop/uploading_updating_mod/){target=_blank}