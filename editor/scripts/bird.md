# Bird

!!! info "v1.4.8"

A skeletal bird that idles on a perch and, if allowed, takes off, flies, and lands. The entity needs a skeleton.

Find placed ones in the Scene panel with `s:Bird`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the bird entity, not on the perch. The entity needs `bird_skeleton` and a bird mesh (`crow_mesh`, `hawk_mesh`). Point the entity the way it should fly. Flight moves it backward along its forward axis.

## What you set

`Native\Prefabs\editor_spawnpoints.xml` stores this order on `crow`.

| Panel field | What to do with it |
|---|---|
| **_flyingSpeedInKph** | Flight speed, in kilometres per hour. The crow stores `14.4`. |
| **_idleAnimation** | Perch clip. The crow stores `anim_bird_idle`. |
| **_landingAnimation** | Landing clip. The crow stores `anim_bird_landing`. |
| **_takingOffAnimation** | Takeoff clip. The crow stores `anim_bird_flying`. |
| **_flyCycleAnimation** | Airborne loop. The crow stores `anim_bird_cycle`. |
| **CanFly** | On: the bird leaves the perch. The crow stores on. A hawk on a stand stores off. |
| **_canLand** | On: an airborne bird lands again. Off: it is hidden when the airborne timer ends. The crow stores off. |

## What it does

It starts perched, unless **CanFly** is on and **_canLand** is off, in which case it starts in the air. From the perch it waits, takes off, flies, then either lands or disappears. A hawk prop that only stores **CanFly** off stays on the idle clip.

## Where it lives

Managed script. Perch props are in `Native\Prefabs\building_details.xml`. The flying crow is in `Native\Prefabs\editor_spawnpoints.xml`. Multiplayer scenes and a few campaign scenes store the script as well.

## Examples

**Crow that does not land.** `Native\Prefabs\editor_spawnpoints.xml`, entity `crow`, skeleton `bird_skeleton`, mesh `crow_mesh`. Speed `14.4`, **CanFly** on, **_canLand** off.

**Hawk on a stand.** `Native\Prefabs\building_details.xml`, child `hawk` of `hawk_stand_a_with_hawk`. The script stores only **CanFly** `false`, so the bird stays on `anim_bird_idle`. The same file's `hawk_cage_a_with_hawk` does the same.
