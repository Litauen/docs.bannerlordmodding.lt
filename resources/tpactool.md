# TpacTool

[TpacTool](https://github.com/szszss/TpacTool) - an open source asset explorer which can open TPAC format files, view and export the contents. [Fork](https://github.com/hunharibo/TpacTool).

* [TpacTool for 1.3+](https://github.com/hunharibo/TpacTool/releases)

## Model export

Before exporting a model, you need to [find it's mesh name](/modding/items/#how-to-find-itemid).

Point TpacTool to the BL folder:

![](/pics/2410020910.png)


Select Model and enter the mesh's name for the model you are searching (sturgia_helmet_c in example), select it in the found results:

![](/pics/2410020911.png)


If it's Rigged, select Rigged, Human skeleton (if it's human, could be horse, etc). Select if you need textures and then export:

![](/pics/2410020911b.png)


!!! warning ".obj files often are not imported into Blender properly. Try .dae instead. Select in the file window -> Save as type: COLLADA (*.dae)"


!!! danger "If crashing - Try to export without textures and with all the LODs."



## Textures

Example exported textures:

![](/pics/2410081307.png)

Note the normals _n texture in ugly-yellow. For some reason it's inverted in the BL.

You can't import it into the Substance for example.

To fix that, open it in the Photoshop ant invert it back with CTRL+i:

![](/pics/2410081508.png)


## Animations

Can't export an animation I want to export. How do I do it? Always get this error:

![](/pics/2604020815.png)

You can only export animations from the vanilla assets because the human skeleton has to be present for it. Mod assets usually don't package that so it's not possible to export anims from other mods.

Workaround: Just put the human.tpac from native in the same folder as the assets and you should be good to go. Don't forget to remove it afterwards, though, so the game won't load it.



## Vertex Paint

!!! warning "Tpac tool changes the vertex paint, which in case of crossbows need to be blue instead of red."

![](/pics/2508062236.png)

[Video](https://www.youtube.com/watch?v=6hG_6RDqgFg)


