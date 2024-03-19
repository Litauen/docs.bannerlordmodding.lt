# Color Grade

!!! quote "[npc99.](https://discord.com/channels/411286129317249035/761302555308720148/1219178270314991667):"

The campaign map uses regional colourgrades controlled by a colour grade grid and colour grades for different times of the day set in campaign_map.xml interpolated atmospheres, which can now be modded.

copy ...\Modules\Native\Atmospheres\Interpolated\campaign_map.xml into ...\Modules\YourModName\Atmospheres\Interpolated and edit as required.

For the time based colour grades edit this last section of campaign_map.xml

![](/pics/2403181249.png)

You can add other times if you want to split the day up differently.

The red channel values in the texture worldmap_colourgrade_grid are linked to regional colour grades via worldmap_color_grades.xml

![](/pics/2403181250.png)

![](/pics/2403181251.png)

Main_map also has one cube map and one scatter map for each hour of a 24 hour cycle, which can be edited in paint.net you can save cubemaps from the editor.

