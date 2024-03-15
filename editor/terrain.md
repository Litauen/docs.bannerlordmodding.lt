# Terrain

## Links

- [Official Guide - Terrain Creation](https://moddocs.bannerlord.com/editor/scene-editor/terrain_creation/)
- [Basic World Mapping](https://docs.google.com/document/d/1npGJ9p1ySdu2RDU19P_2aE-OCsKWie_G02vcws36UIs/edit)
- [Comment créer une World Map](https://docs.google.com/document/d/1vagBrp22ctZs4nFdPNdPCdGnMXvAJLhW2-Eitcq0AyA/edit#heading=h.31ftrq6om0ea)

## Create

![](/pics/UE3sZpn.png)

When creating new terrain you will see:

![](/pics/yytdVZT.png)

### Node/size/dimension?

All scenes are created on a grid of nodes, allowing different levels of detail (LODs) to be generated in surrounding nodes, optimising performance. 

X and Y are the number of nodes to be generated on each 2D axis. 


**Node Size**

Single node size refers to the length of each side of a square node. It is measured in meters or a game-specific equivalent unit.

**Single Node Dimensions**

This represents the number of vertices (points) to be used on each side of each node to create the terrain mesh.
The more vertices, the higher the resolution and detail of the terrain, but it also requires more computational resources.

![](/pics/ZiL27AD.png)

The default size of Calradia (Bannerlord's original map) are 16x16 nodes, 53 metres long and 256x256 vertices.

So, its mesh was generated using 4,096x4,096 vertices (16 x 256 = 4,096), allowing a 4K heightmap to import one pixel of height data into each vertex. Size stretches that triangular mesh over 848 metres x 848 metres (16x53 = 848).

Metres are the game equivalent in a normal mission scene. A world map is a third of that size (i.e the player’s world-map icon will be a third the size of his character in a scene). So horizontally, in human terms Calradia is really only 2.5Kms x 2.5Kms (848x3 = 2,544), but symbolically represents a continent. This makes satellite heightmaps awkward in world-maps as any mountain range must be several miles high to match the player icon and symbolic map.


## Resize

- [Official Guide - Resizing Terrain](https://moddocs.bannerlord.com/editor/scene-editor/terrain_resize/)



## Dark new terrain

!!! quote "TurqoiseTurtle: I deleted the main map terrain and just generated a new terrain and this happens, its completely dark and no paint layer is visible"

![](/pics/2403140806.png)

!!! quote "Swan [ADOD/IAF]: go to your vista texture and rename the tileset to 'none' then add a basic white albedo texture in the vista category"