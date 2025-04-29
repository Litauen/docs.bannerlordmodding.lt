# Editor

## Versions

- Crashes - 1.1.4/1.1.6/1.2.8
- Works (with problems) - 1.2.5beta/1.2.7


## Bugs

* ["Start Mission" does not work](https://forums.taleworlds.com/index.php?threads/v1-2-7-test-function-in-scene-creator-start-mission-dont-work.460937/) ([Testing Custom Scene Ingame](https://docs.google.com/document/d/1Rwsd9pdv5QA5s3K4oOuJX16_K9A5NaoWh0p78IcUi1w/edit?tab=t.0))

## Controller cursor

![](/pics/2402110846.png)

If you see this cursor and you can't access any menus - make sure to not have any controller plugged in while using the Editor. (Xbox controller or any other kind).

## Notes

- Editor needs to be on the same disk as the Game, as the modding tools use the base games files - they both install to the same folder, and that only works if they are on the same disk
- On map save overwrites settlements.xml (crashes if this file is not present or has errors)
- On map save Editor does not write/update/generate settlements_distance_cache.bin

!!! quote "NPC99: Saving uses extra memory. So, sometimes if you are over 80% use of your RAM its worth waiting for 10 minutes or so for the idle Editor to release some memory before saving."


### High CPU usage

In the Visibility tab turn off Game Entities, Helpers and Shadows

![](/pics/B6B7BMC.png)

Looking down to the ground also helps, so no entities will be visible.

<br>
## Problems/Solutions


!!! tip "Use [ButterLib](/modules/mods_for_devs) or [attach dnSpy](/resources/dnspy) to the game to catch the crashes. Do not use both at the same time, dnSpy gets confused."

### Does not start

- If installed on another drive than the main game it will not start
- Version mismatch with the base game - does not start at all, no process - install same version (not always true)
- Hanged previous process - Close on Steam or check the Task Manager, kill the old process

!!! info "Install [Visual C++ Redistributable Packages for Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=40784)"
    ![](/pics/2504280817.png)


### MSVCP120.dll was not found

![](/pics/2403231933.png)

??? info "Install [Visual C++ Redistributable Packages for Visual Studio 2013](https://www.microsoft.com/en-us/download/details.aspx?id=40784)"
    ![](/pics/2504280817.png)


### Closes when Play button is clicked

This function is broken for a long time. Do not use it. 

* ["Start Mission" does not work](https://forums.taleworlds.com/index.php?threads/v1-2-7-test-function-in-scene-creator-start-mission-dont-work.460937/)
* [Testing Custom Scene Ingame](https://docs.google.com/document/d/1Rwsd9pdv5QA5s3K4oOuJX16_K9A5NaoWh0p78IcUi1w/edit?tab=t.0)


### Top Bar gone

![](/pics/2411250838.png)

Delete `..Documents\Mount and Blade II Bannerlord\Editor Files\Layouts`

### Editor Crashes

#### Crash when trying to import PNG texture

Sometimes shows this before crashing:

![](/pics/2408301318.png)

Reason: non-latin letter(s) in the folder's name from which PNG is imported


#### Crash when deleting used texture

After crash/restart Texture appears deleted

#### Crash on the map save

- settlements.xml missing from /MOD/ModuleData folder

- When there is some error in the XML file (settlements.xml)
    Double < Settlements/> for example

- Last line in the log: opening ../../Modules/MODULE_NAME/ModuleData/settlements.xml - missing settlements.xml

#### Crash on the interior scene save

!!! quote "[hunharibo:](https://discord.com/channels/411286129317249035/761302555308720148/1202691179896897536)"
    PSA: for scenes crashing on saves - add a terrain, even if your scene is interior and you would normally not need it<br>
    can be really small, like 2x2, 16 node dimension<br>
    and just hide it away in a corner of the map or put entities on top of it<br>
    terrain present in scene = no crashing on save

#### Crash when deleting the paint layer

No solution yet. Can't delete the layer...

Log shows: [22:19:02.024] rglTerrain_shader_generator::handle_mesh_blend_state : 0.012403

#### Crash on exit

Reason: ??

### Game crashes

- Not fully saved map - could be the reason that game was active/loaded/in progress when map was saved. Exit the game, then save the map in the Editor. Check map folder, should see following files:

![](/pics/VYlzH6c.png)


### Hangs on map save in the Editor

Possible causes:

- Too much terrain editing (Smoothing?)
- Erasing a lot of info on some layer

![](/pics/MMmqYCn.png)

To cancel the Save, use X from the Taskbar:

![](/pics/qOfC2xV.png)

Try to save again.

NOTE: after the save Editor often crashes. (At least you saved your last changes)


### Controls stops to work

Reload/Restart?


## Assert Spam

Spams ASSERT windows on start.

### FairyTale assert

![](/pics/2404120847.jpg)

This one because in file project.mbproj <file> entry was above <Module> entry:

Good:

``` xml
<base type="solution">
    <Module id="soln_physics_materials" name="ModuleData/physics_materials.xml" type="physics_material"/>
    <file id="soln_face_animation_records" name="ModuleData/NoBigSmiles.xml" type="face_animation_record" />
</base>
```

Bad (with assert spam):

``` xml
<base type="solution">
    <file id="soln_face_animation_records" name="ModuleData/NoBigSmiles.xml" type="face_animation_record" />
    <Module id="soln_physics_materials" name="ModuleData/physics_materials.xml" type="physics_material"/>
</base>
```

## Distance Tool

If this appears:

![](/pics/2504280827.png)

Fear not, turn it off with:

![](/pics/2504280828.png)
