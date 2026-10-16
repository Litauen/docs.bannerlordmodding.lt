# path_converger

!!! info "v1.4.8"

Ties a sound entity to a path. **Path Name** is the path. **Rotate To Camera** and **Apply Scale** are checkboxes.

Find placed ones with `s:path_converger`. See [Filter](/editor/editor/#filter).

## Where you put it

Put it on the sound entity, next to its sound script. Leave **Path Name** empty when the sound stays put.

## What you set

Native panel. `lake_ambient_sound` stores this order. `arena_path_sound` omits **Apply Scale**.

| Panel field | What to do with it |
|---|---|
| **Path Name** | Path id. Empty on the lake and arena-path prefabs. |
| **Rotate To Camera** | Checkbox. Opened placements store off. |
| **Apply Scale** | Checkbox. The lake prefab stores off. A town lake sound turns it on. |

## What it does

The entity stores the path id and the two checkboxes. `arena_reactions_sound` names `arena_sound_path`. The town lake sound names `lake_sound_path` and turns **Apply Scale** on.

## Where it lives

Native script. Prefabs are in `Modules\Native\Prefabs\sound_prefabs.xml`.

## Examples

**Lake ambience, no path.** `sound_prefabs.xml`, entity `lake_ambient_sound`. **Path Name** empty, **Rotate To Camera** off, **Apply Scale** off.

**Arena path sound, scale row omitted.** `sound_prefabs.xml`, entity `arena_path_sound`. **Path Name** empty, **Rotate To Camera** off. **Apply Scale** is not stored. The sound event is `event:/mission/ambient/detail/arena_cheer`.

**Arena reactions on a named path.** `sound_prefabs.xml`, entity `arena_reactions_sound`, tag `arena_sound`. **Path Name** `arena_sound_path`. Both checkboxes off.

**Town lake, scale applied.** `SandBoxCore\SceneObj\empire_town_a`. Entity `detail_waves_lake_sound`, scaled to `25`. **Path Name** `lake_sound_path`, **Rotate To Camera** off, **Apply Scale** on.
