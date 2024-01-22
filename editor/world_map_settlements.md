# World Map Settlements


## Entity limit in the scene

[There is a limit around 512k for game entities](https://forums.taleworlds.com/index.php?threads/is-there-a-game_entity-limit-for-scenes.455430/). Adding more leads to a crash.


## Temporary Vista with Settlement placement

I am using temporary vista with locations painted on it for better settlement placement:

![](https://imgur.com/7Nc7oZ9.png)

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

![](https://imgur.com/5DzYkaU.png)

Settlement is deleted, but it's decals remain:

![](https://imgur.com/HHj7nPV.png)

I was not able to find avay to get rid of them. They are gone when the map is reloaded (save-load).

!!! danger "Do not forget to delete it from the settllements.xml - game will crash otherwise"

!!! danger "If you are using CUSTOM_settlements.xml - delete from this file also :)"