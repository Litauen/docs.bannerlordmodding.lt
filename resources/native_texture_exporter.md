# NativeTextureExporter

!!! info "Primarily intended as a quick fix for black textures that appear in custom scenes."

[https://github.com/JoeFwd/Bannerlord.NativeTextureExporter](https://github.com/JoeFwd/Bannerlord.NativeTextureExporter) by Eagle (Joël Forward)

Exports native Bannerlord textures referenced in custom material files of a mod.

A command‑line utility that extracts native game textures referenced by custom material files in a mod, saving them to a folder for import. After running the tool, use the usual modding workflow to scan for changes and import the exported textures into the mod. 

## What it does

* Reads native asset files from the game's native assets directory.
* Scans the specified mod folder for material definitions.
* Identifies texture references in those materials that come from the native assets.
* Copies the required native textures (DDS or PNG) into <ModFolder>/AssetSources/vanilla_texture_reimports.
* Ignores textures that have already been exported to avoid duplicates.


## Workflow

* Drop [`Bannerlord.NativeTextureExporter.exe`](https://github.com/JoeFwd/Bannerlord.NativeTextureExporter/releases) to TPacTool's folder near TpacTool.exe
* Open `Command Prompt` or `Windows PowerShell` and `cd` to TpacTool's folder
* Run:

```
.\Bannerlord.NativeTextureExporter.exe "D:\SteamLibrary\steamapps\common\Mount & Blade II Bannerlord\Modules\Native\EmAssetPackages" "D:\SteamLibrary\steamapps\common\Mount & Blade II Bannerlord\Modules\YOUR_MOD\Assets"`
```

(change paths accordingly)

* No output for some time, then it will produce:

```
Finding materials in folder 'D:\SteamLibrary\steamapps\common\Mount & Blade II Bannerlord\Modules\YOUR_MOD\Assets'
Found material 'lt_mortas_crown_with_wimple'
Found material 'lt_coat_of_plates_black'
Found material 'lt_coat_of_plates_grey'
Found material 'lt_coat_of_plates_piast'
Found material 'lt_coat_of_plates_pomerania'
...
Found material 'lt_saddle_dark2'
Exporting texture 'banner_battania_n' into D:\SteamLibrary\steamapps\common\Mount & Blade II Bannerlord\Modules\YOUR_MOD\Assets\..\AssetSources\vanilla_texture_reimports\banner_factions
Exporting texture 'banner_battania_s' into D:\SteamLibrary\steamapps\common\Mount & Blade II Bannerlord\Modules\YOUR_MOD\Assets\..\AssetSources\vanilla_texture_reimports\banner_factions
Skipping: banner_battania_n has already been exported
...
```

* In `\YOUR_MOD\AssetSources\vanilla_texture_reimports` you will find exported native textures:

![](/pics/2512011320.png)

* Now you can go to the Editor and reimport the textures.

!!! warning "To be safe, I changed the texture names, imported and assigned them to the custom materials."
    Maybe it is not necessary, but it is better not to overwrite the native textures - in my understanding if TW will change them, the change will not be visible with your overwrites.