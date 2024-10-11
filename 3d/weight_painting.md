# Weight Painting

Weight painting defines how much a bone's movement will influence the shape of the mesh at different points.

Weight painting is a technique used in rigging to assign influence to specific parts of a 3D model by painting weights onto the vertices of the model. These weights determine how much each bone in the rig affects the deformation of the model at any given vertex. 

<center>
![](/pics/weight_painting.gif)
</center>


## Guides

* [Lykon Development - Simple Weight Painting using Blender](https://www.youtube.com/watch?v=lW_3gppZ1zo){target=_blank}



## Weight Transfer

* [Gulag's script for Weight Transfer and LODs](/editor/gulags_avto_weightlods_script)

!!! quote "Ragnar Hrodgarson:"

    When it is a simple Armor, for example only cloth or leather and if it is plate not much overlapping parts you could just transfer weights and then make small fixes here and there, mostly in the armpits, or depending if there are belts or other stuff in the hip area.

    For every Armor that is more complex, that could potentially be used as base to start with, but sometimes it is just better and faster to make the weightpaint completely yourself then.

- [Demo](https://drive.google.com/file/d/1_-RlnK4JJmhdvSDamxk27026R1Ksx00a/view) by Ragnar Hrodgarson
- [Demo](https://youtu.be/j8EPTSVhaY8?list=PLjnD9iTZKI9yWnn10FcHImWeYbqbqLIbg&t=503) by Carolina Warlord Gaming
- [Demo1](https://youtu.be/hTCutq0kPJk?t=236) / [Demo2](https://youtu.be/WueD_-nPQ-4?t=20)  by The Lone Warrior


## Check slightly affected areas

![](/pics/2405220825.png)


## Weight Paint whole mesh at once

1. Select your mesh
2. Select 'Edit Mode'
3. Select whole mesh with Select - All (A)
4. Go to Object Data properties
5. Select the Bone
6. Enter the desired weight
7. Assign
8. Check with 'Weight Paint' mode

<center>
<video width="843" height="654" controls autoplay loop muted>
    <source src="/pics/weight_paint_whole_mesh_at_once.webm" type="video/webm">
    Your browser does not support the video tag.
</video>
</center>

## Examples

### Helmet with Aventail

![](/pics/2408301900.png)

### Shoes with 4 bones

![](/pics/2408301902.png)

### Gloves with 3 bones

![](/pics/2408301906.png)


## Problems

### Bad Weight Painting

<video width="389" height="422" controls autoplay loop muted>
    <source src="/pics/2410102105.webm" type="video/webm">
    Your browser does not support the video tag.
</video>

Redo the Weight Painting. Fastest way - do Weight Transfer.
