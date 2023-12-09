# Editor

## Versions

- Crashes - 1.1.4/1.1.6
- Works - 1.2.5beta


<br>
## Notes

- On map save overwrites settlements.xml (crashes if this file is not present)
- On map save does not write/update/generate settlements_distance_cache.bin


<br>
## Problems/Solutions

### Does not start

- Version mismatch with the base game - does not start at all, no process - install same version
- Hanged previous process - Close on Steam or check the Task Manager, kill the old process

### Crashes

- When deleting used texture. After crash/restart Texture appears deleted

#### Map crashes on game load

- Not fully saved map - could be the reason that game was active/loaded/in progress when map was saved. Exit the game, then save the map in the Editor.

- TaleWorlds.CampaignSystem.GameComponents.DefaultMapDistanceModel.GetDistance(Settlement fromSettlement, Settlement toSettlement) - wrongly assigned Settlement, Kingdom without a settlement. Attach dnSpy, it will show faction.FactionMidSettlement == null



#### Crash on the map save in the Editor

- When there is some error in the XML files (settlements.xml?)

- Last line in the log: opening ../../Modules/MODULE_NAME/ModuleData/settlements.xml - missing settlements.xml


### Hangs on map save in the Editor

- Too much terrain editing (Smoothing?). Export heightmap before trying to save after a lot of Smoothing.

![](https://imgur.com/MMmqYCn.png)

To cancel the Save, use X from the Taskbar:

![](https://imgur.com/qOfC2xV.png)

Try to save again.

### Controls stops to work

Reload/Restart?

