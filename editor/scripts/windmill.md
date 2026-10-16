# WindMill

!!! info "v1.4.8"

Spins the entity about its forward axis. **isWaterMill** gates splash particles on numbered children.

Find placed ones with `s:WindMill`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the fan or the wheel. A higher **rotationSpeed** turns faster. The Sturgia fan stores `100`. A street-lamp spinner stores `6000`.

## What you set

`sturgia_windmill_fan_a_open` stores this order. **testEntity** is a public field that prefab does not write.

| Panel field | What to do with it |
|---|---|
| **rotationSpeed** | Spin speed. Class default `100`. |
| **waterSplashPhaseOffset** | Shifts the splash window around the turn. Default `0`. |
| **waterSplashIntervalMultiplier** | How long each child's splash stays on. Default `1`. |
| **testMesh** | Adds that mesh to the entity. Opened prefabs leave it empty. |
| **testTexture** | Sets the diffuse on the first mesh. Opened prefabs leave it empty. |
| **testEntity** | On the panel. Changing it does not add a mesh or a texture. Prefabs do not store it. |
| **isWaterMill** | On: children whose names end in a number run their particles only during their slice of the turn. Off: spin only. |

`sturgia_windmill_fan_a_close` stores the same rows in reverse order, still with **isWaterMill** off. No opened prefab stores **isWaterMill** true.

## What it does

The entity keeps turning in the editor and in a mission. With **isWaterMill** on, a child named with a trailing number (the Battania mill has `prt_watersplash_0`) is the slot index. Particles on that child play only inside its phase window.

## Where it lives

Managed script. Fans are in `Modules\Native\Prefabs\archhitecture_sturgia.xml`. The water wheel is in `Modules\Native\Prefabs\archhitecture_battania.xml`.

## Examples

**Sturgia fan.** `archhitecture_sturgia.xml`, `sturgia_windmill_fan_a_open`. **rotationSpeed** `100`, splash offset `0`, interval `1`, test mesh and texture empty, **isWaterMill** false.

**Battania water wheel, splash gate off.** `archhitecture_battania.xml`, child `battania_watermill_mill`. Same numbers, **isWaterMill** false, and a child `prt_watersplash_0`.

**Fast lamp spinner.** `NavalDLC\Prefabs\nord_town_houses.xml`, prefab `nord_street_lamp_bb`, child `orientation_fix`. **rotationSpeed** `6000`. The test mesh and texture rows are omitted. **isWaterMill** false.

**Burning mill, speed only.** `Native\SceneObj\mp_tdm_map_001`. Prefab `windmill_burning_missile` stores only **rotationSpeed** `600`.
