# GulagEnabler' s Avto-WeightLODs.py script

Made by [GulagEnabler🍉](https://discord.com/channels/411286129317249035/697071240015380500/1216758524055519242)

Script simplifies [Weight Transfer](/editor/weight_painting/#weight-transfer) and LOD generation for the models in Blender.

## Install

Download from [here](https://drive.google.com/file/d/1T2-PSjuO_eY_Dgpvq9Mxj13xysPa8f3q/view?usp=drive_link)

Install into Blender:

![](/pics/2403202131.jpg)

![](/pics/2403202132.jpg)

At the bottom should show:

![](/pics/2403202134.jpg)

Enable the addon:

![](/pics/2403202136.jpg)

Press N and click on Tool to see the addon window:

![](/pics/2403202137.jpg)


## Weight Transfer

Select source mesh (with LMB) - from where to transfer weights

Select destination (target) mesh (with ctrl+LMB) - where to transfer the weights

Press "Weight Paint":

![](/pics/2403202140.jpg)

Select (LMB) only the target mesh.

This DataTransfer window should appear for the target mesh:

![](/pics/2403202141.jpg)


??? failure "If it does not appear, delete all Vertex Groups from the target mesh and repeat the previous step (click on Weight Paint):"
    ![](/pics/2403202143.jpg)

Press on Generate Data Layers:

![](/pics/2403202144.jpg)

Apply the modifier:

![](/pics/2403202145.jpg)

(For target mesh) check if Vertex Groups are created:

![](/pics/2403202146.jpg)

and are they weight painted:

![](/pics/2403202146b.jpg)

Delete non-active groups, that do not impact the mesh (all blue):

![](/pics/2403202146c.jpg)


### Demo

![](/pics/2403202147.gif)

![](/pics/2403202152.gif)

## Delete Empty Vertex Groups

Press on "Delete Empty Vertex Groups" to remove empty groups.

![](/pics/delete_empty_vertex_groups.gif)


## LODs

Select your mesh. Make sure it has correct name (which you will not be able to change in the BL Editor)

Newly generated LOD starts from 0.8 Decimate Ratio, then 0.4, 0.2, 0.1, 0.05 ...

Set how many LODs to generate and press the "Generate LODs" button:

![](/pics/2403202149.jpg)

### Demo

![](/pics/2403202153.gif)
