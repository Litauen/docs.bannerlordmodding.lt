# Custom Main Menu

<center>
![](/pics/initial_screen_demo.gif)
</center>

You can create any scene here, my intention was to change it slightly using the same model with cloth physics. As far as I know - custom imported models do not support cloth physics (I may be wrong here).

## Workflow

Provided [here](https://discord.com/channels/411286129317249035/761302555308720148/997136349289267260) by CarolinaWarlord[ROT]:

    1.) extract intro_scene model with TPAC.
    2.) delete faces and materials you want to replace.
    3.) import your new equipment pieces and sculpt into position.
    4.) join your sculpted equipment meshes with the intro_scene model.
    5.) export to your module, import into mod tools and assign materials.
    6.) open main_menu_a scene in mod tools.
    7.) orient your intro_scene model and camera angle how you want it.
    8.) save the scene as main_menu_a to your module

## Model

Export native model with textures using the TpacTool:

![](/pics/2402261842.png)

You will get a lot of files:

![](/pics/2402261848.png)

Import FBX into the Blender:

![](/pics/2402261844.png)

It consists of separate parts, you can hide/remove them if you dont need them. I removed shoulder-fur and the helmet.

It is possible to edit meshes with Blender. I removed skirt-things from the native model:

![](/pics/2402261850.png)

I added new helmet in the Blender. Make sure it's parent is set to the intro_scene_node.

![](/pics/2402261853.png)

The problem with the new helmet is that it's static. Other meshes are using 150 Shape Keys for movement, but I was unable to get any movement using them:

![](/pics/2402261859.png)

!!! quote "Le Profyteur: after you can make a animation on blender and export with your mesh, after you can add a play anim script to play the clip"

When happy with the changes, export FBX and import it into the Editor.

Open

    \Modules\Native\SceneObj\main_menu_a

and place your model there. 

??? danger "MAJOR PROBLEM: Editor fails to start"

    For me on the start the Editor is throwing zillions of such errors when 'intro_scene' model is imported:

    ![](/pics/2402261945.png)

    So basically the Editor becomes unusable. My "solution" was to banish this model with related files to the separate mod :( If you know how to fix that - please let me know. Converting/fixing the model in the Blender will remove cloth physics from it, so not a solution in my case.

    !!! info "To make it start, delete intro_scene_geo.tpac file from your mod's /Assets folder (~55mb)"

### Movement

To activate the cloth physics add VertexAnimator Script and set it like this:

![](/pics/2402262008.png)


## Textures

To further modify the model, use extracted textures to change the look. I used them to make this guy look like a Teutonic Knight:

![](/pics/2402261950.png)

Import texture files with the same names, and new textures will be applied automatically.

!!! warning "Renaming the texture and creating a custom material will disable cloth physics"

## Scene saving

Save as a new scene with the same name main_menu_a in your mod's folder. It will be used on the next game start.

!!! bug "In 1.2.9 if you try to save (Save as...) this scene, the Editor will crash."

    Add a terrain to avoid this:

    ![](/pics/2402262011.png)


## Window view tuning

The look of the menu can be edited in the prefab file:

    \Modules\Native\GUI\Prefabs\InitialScreen.xml

and in the brush file:

    \Modules\Native\GUI\Brushes\InitialMenu.xml

I added slight transparency on the black background under the menu.

## Other Notes

* Change the scene as you like it. Use useful [camera_instance controls](/editor/scenes/#camera-instance) to see the changes in real time.

* Custom Music in Main Menu guide [here](/modding/sound/#custom-music-in-main-menu)

!!! quote "Bullero: some effects animations scripts not work in menu scenes"


## Related mods

* [True Menu Refinement Tool](https://www.nexusmods.com/mountandblade2bannerlord/mods/5233)
* [Alternating Main Menu Background](https://www.nexusmods.com/mountandblade2bannerlord/mods/2885)
