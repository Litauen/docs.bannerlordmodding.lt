# Export to FBX (from Blender)

## Select all elements

If the mesh is rigged (armor/helmet/boots/gloves/shoulders) - then you need to select with human_skeleton_notused, like this:

![](/pics/2409281004.png)

For the shields select two bo_.. elements and main mesh with lods:

![](/pics/2409281005.png)


## File - Export - FBX

![](/pics/2409281009.png)


## Select export settings

Check Limit to Selected Objects:

![](/pics/2409281011.png)

Uncheck Add Leaf Bones:

![](/pics/2409281012.png)

Leave other settings as they are.


## Export

![](/pics/2409281016.png)

## Create preset

Create preset for the future to avoid clicking those settings every time:

![](/pics/2409281017.png)


## Problems

### Add Leaf Bones enabled

You will get this in the [Model Viewer](/3d/model_viewer/):

<video width="395" height="325" controls autoplay loop muted>
    <source src="/pics/add_leaf_bones.webm" type="video/webm">
    Your browser does not support the video tag.
</video>

![](/pics/2409281039.png)


### Not visible in the Inventory

![](/pics/2411221718.png)

Reason #1: Wrong mesh in the item description:

![](/pics/2411221720.png)

Reason #2: Model is too big in the Blender:

![](/pics/2411221718b.png)

After reimport with proper size:

![](/pics/2411221718c.png)
