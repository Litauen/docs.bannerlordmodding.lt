# Editor

## Versions

- Crashes - 1.1.4/1.1.6/1.2.8
- Works (with problems) - 1.2.5beta/1.2.7


<br>
## Notes

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

- Version mismatch with the base game - does not start at all, no process - install same version
- Hanged previous process - Close on Steam or check the Task Manager, kill the old process

### Editor Crashes

#### Crash when deleting used texture

After crash/restart Texture appears deleted

#### Crash on the map save

- settlements.xml missing from /MOD/ModuleData folder

- When there is some error in the XML files (settlements.xml?)
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




