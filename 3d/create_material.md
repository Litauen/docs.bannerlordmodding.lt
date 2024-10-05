# Create Material

## Open Resource Browser

Open the Modding Kit ([Editor](/editor/editor/)) and then [Resource Browser](/editor/resource_browser/) with your mod selected:

![](/pics/2410040940.png)

Go to `Modules - YOUR_MOD - Assets`:

![](/pics/2410040914.png)

## Create Folder

RMB (Right Mouse Button) on the empty space and `Create - Folder`:

![](/pics/2410040916.png)

Name it like your project. For example I call it 'lt_hrodno_helmet':

![](/pics/2410040917.png)

## Import Textures

Go inside that folder, RMB on the empty space, `Import new asset':

![](/pics/2410040919.png)

Select the textures for your model and press Open:

![](/pics/2410040920.png)

Click through the usual error messages:

![](/pics/2410040924.png)

Result should look like this:

![](/pics/2410040943.png)


??? failure "Editor crashes, PNG is not imported:"
    Sometimes shows such error message before crashing:<br>
    ![](/pics/2410041152.png)<br>
    Reason: There are non-Latin characters in the folder's name where the PNG files are located. ¯\\\_(ツ)\_/¯

## Create Material

RMB on the empty space, `Create - Material`:

![](/pics/2410040947.png)

Name it like your project name:

![](/pics/2410040948.png)


## Assign Textures

In the right panel click on the `default_editor_texture_d` in the `DiffuseMap` and select your first texture that usually ends in _d (diffuse):

![](/pics/2410040951.png)

`NormalMap` and `SpecularMap` textures are assigned automatically, if you have textures with the same name with _n and _s at the end of the names:

![](/pics/2410040952.png)


## Set Settings

In `Material Shader Flags` mark `use_specular`, `do_not_use_alpha` (if you are not using alpha/transparency), `do_not_use_vertex_color_as_occlusion`:

![](/pics/2410040955.png)


In `Others` set `Two Sided` if you need texture to be visible from both sides (I do this for helmets so the insides could be visible also), in `Vertex Layout` mark `Skinning` if your model is rigged ([Weight Painted](/3d/weight_painting/) with [Armature/Skeleton](/3d/armature_skeleton/)), then press Save:

![](/pics/2410040956.png)


??? failure "If your model is rigged and you forgot to mark `Skinning` you will get this in the [Model Viewer](/3d/model_viewer/#disabled-skinning):"
    <video width="285" height="269" controls autoplay loop muted>
        <source src="/pics/material_skinning_problem.webm" type="video/webm">
        Your browser does not support the video tag.
    </video>
