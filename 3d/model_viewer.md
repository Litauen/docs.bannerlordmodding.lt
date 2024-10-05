# Model Viewer

* [OFFICIAL: Model Viewer](https://moddocs.bannerlord.com/editor/resource-editors/model_viewer/)

## Open

Editor > Window > Show Model Viewer:

![](/pics/2410042155.png)

From the left panel, you can change Atmosphere, hide/show ground, or add as many entities as you want. The entities can either be Human or simple Mesh. Pressing Add Entity will open a modal window for you to select the entity type:

## Add human

![](/pics/2410042157.png)

To avoid super slow zoom I enlarge my human x10. That allows to look around him much much faster:

![](/pics/2410042158.png)

!!! tip "Holding SHIFT while zooming helps also"


## Add armors

Add armors to the `Body parts` section, not `Items`:

![](/pics/2410042201.png)

## Animation

For quick tests I am using:

* `lord_walk` - `lord_walk_forward` (good for normal movement testing)
* `run` - and the last from the list (easy to set) (good for extreme movement testing)
* `inventory` - `inventory_idle` (good for idle testing, for small details)

![](/pics/2410042204.png)

<br><br>
## Problems

### Add Leaf Bones enabled

<video width="395" height="325" controls autoplay loop muted>
    <source src="/pics/add_leaf_bones.webm" type="video/webm">
    Your browser does not support the video tag.
</video>

Cause: Add Leaf Bones enabled in the Blender: [FBX Export](/3d/export_to_fbx/)

![](/pics/2409281039.png)


### Missing/bad [Armature](/3d/armature_skeleton/)

<video width="580" height="478" controls autoplay loop muted>
    <source src="/pics/fbx_armature_problem.webm" type="video/webm">
    Your browser does not support the video tag.
</video>

Cause: missing [armature (skeleton)](/3d/armature_skeleton/) or wrong armature assigned:

![](/pics/2409281050.png)


### Disabled skinning

<video width="285" height="269" controls autoplay loop muted>
    <source src="/pics/material_skinning_problem.webm" type="video/webm">
    Your browser does not support the video tag.
</video>


Cause: Skinning disabled in material settings:

![](/pics/2409281102.png)


<br><br>
## Model Viewer Problems

### Save/Load Scene

`File` - `Save Scene`/`Load Saved Scene` messes up the viewer, weird colors/shadows, no idea how to restore without the Editor restart:

![](/pics/2410042212.png)

### Closed window

If Model Viewer window is closed and then opened again from the menu - it does not work. 

Helps only Editor restart:

![](/pics/2410042207.png)