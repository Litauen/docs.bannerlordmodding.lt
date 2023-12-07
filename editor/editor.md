# Editor

## Versions

- Crashes - 1.1.4/1.1.6
- Works - 1.2.5beta


## Problems/Solutions

### Does not start

- Version mismatch with the base game - does not start at all, no process - install same version
- Hanged previous process - check Task Manager, kill old process

### Crashes

- When deleting used texture. After crash/restart Texture appears deleted

#### Map crashes no game load

- Not fully saved map - could be the reason that game was active/loaded/in progress when map was saved. Exit the game, then save the map in the Editor.

#### Crash on map save in the Editor

- When there is some error in the XML files (settlements.xml?)

### Hangs on map save in the Editor

- Too much terrain editing (Smoothing?). Export heightmap before trying to save after a lot of Smoothing.

![](https://imgur.com/MMmqYCn.png)

To cancel the Save, use X from the Taskbar:

![](https://imgur.com/qOfC2xV.png)

Try to save again.

### Controls stops to work

Reload/Restart?

