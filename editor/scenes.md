# Scenes

- [Official documentation](https://moddocs.bannerlord.com/authoring-mission-scenes/)
- [Hideout Template with Tutorial and savegame](https://www.nexusmods.com/mountandblade2bannerlord/mods/3036)
- [Testing Custom Scene Ingame](https://docs.google.com/document/d/1Rwsd9pdv5QA5s3K4oOuJX16_K9A5NaoWh0p78IcUi1w/edit)
- [Troubleshooting crashing Siege Scene](/guides/troubleshooting_siege_scene/)

??? quote "Snorri:"
    ![](/pics/2403070727.png)

## Camera Instance

![](/pics/2402262002.png)

Use Show Display to instanly see the camera's view (with Render Skybox on).


## Siege Engines

!!! quote "Snorri - from extensive siege scenes testing from creating them and figguring out how to make AI stupidnes at leest mimic what should be done"

* Siege engines mostly aim for enemy siege engines (or rather siege engine crew), so anything around them could also by targeted by mistake. Placing your troops next to siege engines is not a best idea.
* It seems that after extensive, non effectice fire to enemy siege engines, or if they go beyond reach, or if there any from start, it choose another target.
* Siege engines can aim into singular troops, and it's super weird how precise it could be. Usually they will chose targets that are closer to them, but not always, it dose't seems to me that AI know if it shoot to whole formation or singular troop, so it will not chose to shoot to bigger stack as would be logical for most kills ratio.
* Siege engines need more or less direct sight to target, they can't shoot over walls, hills etc. it also don't care if friendly troops are in a way, that's why most defender siege engines are places nearly always so much on front, they can do sooo much friendly fire if you will give them a chance (as they will chose to shot more nearby enemy that is probably in fight with your troops).
* Siege engines don't seems to care about destructible things as merlons and roofs, they destroy them by mistake trying to target troops/siege engines.

It's all from just extensive observations and testing by changing things in scene editor.


## Various

search for "debug" in the entity browser, place it down and run it's script, there you can choose what type of scene you are making and see the requirements for it in terms of spawners and such
