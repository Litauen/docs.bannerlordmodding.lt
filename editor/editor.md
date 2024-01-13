# Editor

## Versions

- Crashes - 1.1.4/1.1.6
- Works (with problems) - 1.2.5beta/1.2.7


<br>
## Notes

- On map save overwrites settlements.xml (crashes if this file is not present or has errors)
- On map save Editor does not write/update/generate settlements_distance_cache.bin

### High CPU usage

In the Visibility tab turn off Game Entities, Helpers and Shadows

![](https://imgur.com/B6B7BMC.png)

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

- When there is some error in the XML files (settlements.xml?)
    Double < Settlements/> for example

- Last line in the log: opening ../../Modules/MODULE_NAME/ModuleData/settlements.xml - missing settlements.xml

#### Crash when deleting the paint layer

No solution yet. Can't delete the layer...

Log shows: [22:19:02.024] rglTerrain_shader_generator::handle_mesh_blend_state : 0.012403




### Game crashes

- Not fully saved map - could be the reason that game was active/loaded/in progress when map was saved. Exit the game, then save the map in the Editor. Check map folder, should see following files:

![](https://imgur.com/VYlzH6c.png)

??? failure "System.AccessViolationException"
    **at Vec2 SandBox.MapScene.GetNavigationMeshCenterPosition(PathFaceRecord face)**<br>
    **at Settlement ..DefaultMapDistanceModel.GetClosestSettlementForNavigationMesh(PathFaceRecord face)**
    <br><br>
    REASON1: CUSTOM_settlements.xml does not match settlements.xml


??? failure "System.NullReferenceException"
    **DefaultMapDistanceModel.GetDistance(Settlement fromSettlement, Settlement toSettlement)**
    <br><br>
    REASON1: Wrongly assigned Settlement, Kingdom without a settlement. Attach dnSpy, it will show faction.FactionMidSettlement == null. Fix your XML.
    <br><br>
    REASON2: Problems with Navmesh. Inaccessible settlements. Old/mismatching settlements_distance_cache.bin - fix navmesh, regenerate settlements_distance_cache.bin
    <br><br>
    **CampaignObjectManager.InitializeCachedData()**
    <br><br>
    REASON: settlements.xml error - I accidently deleted one settlement and village had no bounded castle (crash on settlement.OwnerClan.OnBoundVillageAdded(settlement.Village);). This was on new game start.

??? failure "System.Reflection.TargetInvocationException"
    **RuntimeMethodHandle.InvokeMethod**
    <br><br>
    REASON: map xscene settlement ID mismatch with settlements.xml settlement ID / settlements.xml has a settlement with ID, which is not present in the xscene file

### Hangs on map save in the Editor

Possible causes:

- Too much terrain editing (Smoothing?)
- Erasing a lot of info on some layer

![](https://imgur.com/MMmqYCn.png)

To cancel the Save, use X from the Taskbar:

![](https://imgur.com/qOfC2xV.png)

Try to save again.

NOTE: after the save Editor often crashes. (At least you saved your last changes)


### Controls stops to work

Reload/Restart?




