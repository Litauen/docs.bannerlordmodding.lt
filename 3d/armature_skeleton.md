# Armature/Skeleton

![](/pics/2410021050.png)

Skeleton is necessary for all the rigged items (body armors/helmets/shoulder coverings/gloves/shoes/horse armors).

Usually model comes without the skeleton.

## Import

[Import this FBX](https://drive.google.com/file/d/1Pasf8ZmngJGP5eKlTIG_JS8LlFG7u42Q/view?usp=drive_link) to get the skeleton and male body at once.

(More models [here](https://drive.google.com/drive/folders/1mi2y_sO-ctpqScMlT5zvU1r01L2810_V?usp=drive_link))

## Rename

Rename the `human_skeleton_notused.00x` to `human_skeleton_notused` and hide the body for now:

![](/pics/2410021056.png)


## Assign

Move (select + drag&drop) all `human_skeleton_notused` to the `Collection`:

<video width="313" height="155" controls autoplay loop muted>
    <source src="/pics/skeleton_to_collection.webm" type="video/webm">
    Your browser does not support the video tag.
</video>


Assign your mesh to the skeleton:

1. Select the mesh with LMB (Left Mouse Button)
2. Select the skeleton with CTRL+LMB
3. Move mouse to the working area ant press CTLR+P
4. Select `With Empty Groups`



<video width="751" height="415" controls autoplay loop muted>
    <source src="/pics/assign_mesh_to_skeleton.webm" type="video/webm">
    Your browser does not support the video tag.
</video>


To be sure that mesh is assigned to the skeleton properly:

1. Select your mesh
2. Go to `Modifier Properties`
3. Make sure Armature - Object is `human_skeleton_notused`

![](/pics/2410021220.png)