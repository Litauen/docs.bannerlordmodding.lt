# World Map Settlements

Settlement icons added without definitions are just meshes no different from rocks. To function as settlements they need all the features required by BL game code. For example towns require a town_gate, bo_town physics sphere, town decal circle, two banner_pos entities per level, two breach wall sets per upgrade level a full siege set with an icon for all levels or icons for each level. Best to copy an existing settlement in total, give it a unique id, delete unnecessary/cosmetic icons and add replacement icons. As well as the settlement id you need a unique town id in components. Other than that the settlement needs to be recomputed into the settlement distance cache for AI pathing.

## Entity limit in the scene

[There is a limit around 512k for game entities](https://forums.taleworlds.com/index.php?threads/is-there-a-game_entity-limit-for-scenes.455430/) / [50k limit?](https://forums.taleworlds.com/index.php?threads/map-entity-limit.460994/#post-9887478). Adding more leads to a crash.

!!! question "What to do?"
    Bubu:<br>
    Yeah only option is to create a Prefab folder in the module folder<br>
    Then select multiple towns/villages/castles, assign them to some capsule<br>
    And then save that capsule as a prefab<br>
    I saved entirity of France for example as a prefab<br>
    Same with multiple other regions


## Temporary Vista with Settlement placement

I am using temporary vista with locations painted on it for better settlement placement:

![](/pics/7Nc7oZ9.png)

- Import tmp Vista into Resource Browser

[PIC]

- Apply tmp Vista to the map

[PIC]

## Workflow to add a new Settlement

- Apply tmp Vista to the map, to easily find the correct spot for the settlement
- Google about the settlement, it's type/style, wooden/brick/stone castle, etc
- Select [what model](/modding/settlements) will represent your settlement
- Clean environment for the settlement if necessary (move away other entities/remove trees/etc)
- Flatten the space for the settlement [LINK TO THE FLATTEN GUIDE]
- Clone the settlement [LINK TO THE CLONE GUIDE]
- SAVE HERE - the Editor crashes sometimes after this step, helps to save effort/time
- Resize the settlement if necessary [LINK TO THE RESIZE GUIDE]
- If castle/town - position/rotate so siege engines would be properly positioned [LINK TO ROTATE GUIDE]
- If village and castle/town nearby - check for siege engines overlapping with the village [LINK HOW TO MANAGE SIEGE ENGINES]
- Improve the settlement (raise/lower/resize/add/remove elements), check main houses/wells/etc, L1 L2 L3 [LINK HOW TO MANAGE TOWN ELEMENTS]
- Improve the map (trees, terrain, grass, dirt, flora, rivers, roads, etc)
- Check Level 1/2/3 separately
- SAVE
- Test in game:
    * Belongs to the correct owner, castle/town if village
    * Correct culture, hearths, prosperity, etc
    * Can enter
    * town_circle_decal present
    * No flying houses, correct level of terrain, other anomalies


## Cloning a Settlement

### Wrong gate position

If after cloning new settlement's gate is at the old settlement's gate - delete [settlement's distance cache](/modding/settlements/#settlements-distance-cache) and allow game to regenerate it.

## Deleting a Settlement

Mark a settlement and press Del or select Delete from the menu:

![](/pics/5DzYkaU.png)

Settlement is deleted, but it's decals remain:

![](/pics/HHj7nPV.png)

I was not able to find avay to get rid of them. They are gone when the map is reloaded (save-load).

!!! danger "Do not forget to delete it from the settllements.xml - game will crash otherwise"

