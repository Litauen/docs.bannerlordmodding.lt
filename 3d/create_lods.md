# Create LODs

```
LOD0 → LOD1    15 m
LOD1 → LOD2    22.5 m
LOD2 → LOD3    30 m
LOD3 → LOD4    50 m
LOD4 → LOD5    70 m
LOD5 → LOD6    130 m
LOD6 → culled/very cheap 210 m+
```

It can be done by hand, but believe me, you will hate doing this over and over and over again manually.

We will use amazing [script made by GulagEnabler🍉](/editor/gulags_avto_weightlods_script/#lods).

Install it and use it. 1 click and it's done:

<video width="810" height="380" controls autoplay loop muted>
    <source src="/pics/gulag_lod_demo.webm" type="video/webm">
    Your browser does not support the video tag.
</video>


## Weapon LODs

!!! quote "PlebeianKilo [ER]:"

    For weapons game uses LOD1 in battle and LOD0 in crafting menu.

    That's why I remove LOD1 for weapons.

    If it looks fine in the crafting menu but off in a scene, remove the LOD1.

![](/pics/202508260949.png)
