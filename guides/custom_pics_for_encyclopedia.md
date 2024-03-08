# Custom pictures for Encyclopedia

<center>
![](/pics/encyclopedia_pics.gif)
</center>

Encyclopedia pictures are in the ui_encyclopedia_1.png. Use TpacTool/BannerEdge to extract them.

Or just download from this [link](https://drive.google.com/file/d/1VRVqGKRkz4OoT-hkDti_UCTAkLP57fLt/view?usp=drive_link).

Extract the archive to your MOD_FOLDER/GUI/ folder

Change the appropriate files in MOD_FOLDER/GUI/SpriteParts/ui_encyclopedia/

[Import the sprites](/gauntletui/sprites/#import) in the Editor.


!!! warning "This method overwrites several sprites and this happens (lines and frames are gone):"

![](/pics/2403072105.png)

To fix it in YOURMOD/GUI/YOURMORSpriteData.xml at the end of &lt;Sprites> add:

``` xml
<NineRegionSprite>
    <Name>troop_tree_side_9</Name>
    <SpritePartName>Encyclopedia\troop_tree_side</SpritePartName>
    <LeftWidth>8</LeftWidth>
    <RightWidth>8</RightWidth>
    <TopHeight>0</TopHeight>
    <BottomHeight>0</BottomHeight>
</NineRegionSprite>
<NineRegionSprite>
    <Name>subpage_slick_frame_9</Name>
    <SpritePartName>Encyclopedia\subpage_slick_frame</SpritePartName>
    <LeftWidth>14</LeftWidth>
    <RightWidth>15</RightWidth>
    <TopHeight>15</TopHeight>
    <BottomHeight>15</BottomHeight>
</NineRegionSprite>
```

This change is overwriten next time you will generate new sprites, so it's annoying, but I don't know better way currently to resolve this problem.