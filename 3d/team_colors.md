# Team Colors

![](/pics/2410082113.png)

## Base Layer

Make sure you have white-light gray color on the parts where you will want team colors to be applied on top (dark base color will mess up the team color):

![](/pics/2410082216.png)

## Mask

Then you will need a mask (diffuse texture). Example:

![](/pics/2410082119.png)

* Black - will leave original texture / no change here
* Red - marks where the game will put primary team color
* Green - secondary team color

## Material

Set this mask as `Diffuse2Map` and check `use_double_colormap_with_mask_texture`:

![](/pics/2410082132.png)

## XML

Then in XML in the [item's](/modding/items/#xml) description make sure you have:

```xml
<Flags UseTeamColor="true"/>
```

## Color Variations

You can give half values to the color mask textures, something like 0.5. Then it will use both the base texture and the faction's team color in a blended way:

![](/pics/2410082152.png)


## With weapons

!!! quote "Sh1ny4: Since 1.3 some lances parts (the banner ones) have team color enabled, no idea of if it can be used on other weapons but at least we have some form of progress"

## Horse Armor Problem

!!! bug "Team colors on horse armors/caparisons are not supported by the game. Bug report [here](https://forums.taleworlds.com/index.php?threads/horse-armors-with-useteamcolor-do-not-work-in-the-game-they-work-in-the-save-game-screen-though.463039/)."
