# TpacTool

[TpacTool](https://github.com/szszss/TpacTool) - an open source asset explorer which can open TPAC format files, view and export the contents. [Fork](https://github.com/hunharibo/TpacTool).

## Model export

Before exporting a model, you need to [find it's mesh name](/modding/items/#how-to-find-itemid).

Point TpacTool to the BL folder:

![](/pics/2410020910.png)


Select Model and enter the mesh's name for the model you are searching (sturgia_helmet_c in example), select it in the found results:

![](/pics/2410020911.png)


If it's Rigged, select Rigged, Human skeleton (if it's human, could be horse, etc). Select if you need textures and then export:

![](/pics/2410020911b.png)


## Textures

Example exported textures:

![](/pics/2410081307.png)

Note the normals _n texture in ugly-yellow. For some reason it's inverted in the BL. 

You can't import it into the Substance for example.

To fix that, open it in the Photoshop ant invert it back with CTRL+i:

![](/pics/2410081508.png)
