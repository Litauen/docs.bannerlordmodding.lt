# Sprites

## Tutorials

* [Generating and Loading UI Sprite Sheets](https://moddocs.bannerlord.com/asset-management/generating_and_loading_ui_sprite_sheets/){target=_blank}
* [Lesser Scholar - Ep 11: Loading UI Sprites. GauntletUI Layouting](https://www.youtube.com/watch?v=TGJe6Ia7_0g&list=PLzebdAxJeltRwfJ8jzsNolgHkRvLjoCRC&index=12){target=_blank}
* [Carolina Warlord Gaming - Adding Custom Sprites](https://www.youtube.com/watch?v=a-UAdVPUimE&list=PLjnD9iTZKI9yWnn10FcHImWeYbqbqLIbg&index=4){target=_blank}

## Import

Put your sprites (PNG files) into folder:

    Modules\YOUR_MODULE_NAME\GUI\SpriteParts\ui_{YOUR_CATEGORY_NAME}


In file:

    Modules\YOUR_MODULE_NAME\GUI\Config.xml

add

``` xml
<Config>
    <SpriteCategory Name="ui_{YOUR_CATEGORY_NAME}">
        <AlwaysLoad/>
    </SpriteCategory>
</Config>
```

to always load your sprite category. [Read here](https://moddocs.bannerlord.com/asset-management/generating_and_loading_ui_sprite_sheets/){target=_blank} if you want to manage loading manually.


Run:

    INSTALLATION_PATH\Mount & Blade II Bannerlord\bin\Win64_Shipping_wEditor\TaleWorlds.TwoDimension.SpriteSheetGenerator.exe

!!! warning "Do not forget to press ENTER to finish the script and unlock the PNG, otherwise it will not be accessible."


Run Mount & Blade II: Bannerlord - Modding Kit from Steam.<br>
Make sure your module is selected in the Mods section of the Launcher, then hit “Play”.<br>
Alt + `

    resource.show_resource_browser

Collapse the folder named Native on the left of the resource browser to see your module easily. Then, select your module.

Open the GauntletUI folder in your module.

![](/pics/G3Xd5ts.png)

press the “Scan new asset files” button which is pointed with a red arrow below

![](/pics/rFDTvn5.png)

??? failure "If nothing happens"

    1. Make sure you ran TaleWorlds.TwoDimension.SpriteSheetGenerator.exe

        SpriteSheetGenerator.exe will create two folders named Assets and AssetSources under Modules\YOUR_MODULE_NAME.<br>
        It will also create a SpriteData.xml file (with a prefix of your module name) under Modules\YOUR_MODULE_NAME\GUI.

        Check these folders/files to make sure they were generated properly.

    2. If you are adding new sprites for the existing category or updating the old ones - make sure you did proper reimport as described below.

![](/pics/PwZr8OQ.png)

Make sure your files are selected then press the Import button. Then, you should see something similar to this:

![](/pics/T5QH7qk.png)

Close the resource browser and the Editor(game). You should now see a new file named ui_{YOUR_CATEGORY_NAME}_1_tex.tpac under Modules\YOUR_MODULE_NAME\Assets\GauntletUI.


## Reimport or add more sprites for the same category

* Put new/updated PNG into GUI\SpriteParts\ui_{YOUR_CATEGORY_NAME}
* Run TaleWorlds.TwoDimension.SpriteSheetGenerator.exe
* In resource.show_resource_browser press RMB on the existing category and select Reimport:

![](/pics/b3kmMwv.png)


## Count sprites in the category

```cs
SpriteCategory sploadingCategory;
if (!UIResourceManager.SpriteData.SpriteCategories.TryGetValue("ui_loading_custom", out sploadingCategory))
{
    // log error ($"Can't find SpriteCategory 'ui_loading_custom'");
    return;
}
int totalGenericImageCount = sploadingCategory.SpriteParts.Count;
```

## NineRegionSprites

![](/pics/Nine_Slice_1_Guides.png)

Quite an interesting way to have strething sprites. Top/bottom/left/right margins can be fixed and what's between - stretched.

Good explanation [here](https://manual.gamemaker.io/monthly/en/The_Asset_Editors/Sprite_Properties/Nine_Slices.htm).

Defined in the sprite file as:

``` xml
<NineRegionSprite>
    <Name>troop_tree_side_9</Name>
    <SpritePartName>Encyclopedia\troop_tree_side</SpritePartName>
    <LeftWidth>8</LeftWidth>
    <RightWidth>8</RightWidth>
    <TopHeight>0</TopHeight>
    <BottomHeight>0</BottomHeight>
</NineRegionSprite>
```


## Possible problems

### Sprite not visible in the game

Make sure there is

``` xml
<AlwaysLoad />
```

in the MODULE_NAMESpriteData.xml under your category.

If it's not, make sure you configured it into Config.xml and category name starts with ui_

Config.xml should be in the SpriteParts folder.

### Sprite resized in a weird way

Possible reason: you added a new sprite, executed TaleWorlds.TwoDimension.SpriteSheetGenerator.exe but failed to Reimport with the Resource Browser.


### Missing texture

![](/pics/eiAJKer.png){ align=left }

Make sure you [imported](https://moddocs.bannerlord.com/asset-management/generating_and_loading_ui_sprite_sheets/#importing-created-sprite-sheets){target=_blank} your sprite into the Resource Browser and tpac file was generated in the Assets or AssetPackages (after Mod publishing) folder.
