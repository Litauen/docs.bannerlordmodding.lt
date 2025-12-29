# Custom Menu Background

<center>
![](/pics/PeVfWv5.png)
</center>

## Custom image

Make custom image, Width x Height: 445x805

[Import as sprite](/gauntletui/sprites/){target=_blank}

## Set as menu background

``` cs
args.MenuContext.SetBackgroundMeshName("YOUR_SPRITE_NAME_HERE");
```

## Other menus

- [Settlements Menu Image](/modding/settlements/#wait_mesh) (wait_mesh in settlements.xml)
- [Encounters](/modding/cultures/#xml) (encounter_background_mesh in cultures.xml)

### Alley/Arena/Keep/Tavern/Port

![](/pics/2402080959.png)

These menu backgrounds are culture related.

They are saved in the /GUI/SpriteParts/ui_fullbackgrounds

Names:

- CULTURE_alley.png
- CULTURE_arena.png
- CULTURE_keep.png
- CULTURE_tavern.png
- CULTURE_port.png (with DLC)

When adding these for your custom culture, make sure you extract/include all the vanila PNGs as well. Otherwise you will be missing a lot of menu backgrounds.