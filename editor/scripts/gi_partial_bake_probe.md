# gi_partial_bake_probe

!!! info "v1.4.8"

A partial GI bake probe. **RayCast Quality** is `Normal` or `High`.

Find placed ones with `s:gi_partial_bake_probe`. See [Filter](/editor/editor/#filter).

## Where you put it

Drop prefab `partial_gi_baker`. It is an editor-only cube (`editor_cube_centered`, visible only while editing).

## What you set

Native panel. The prefab stores the quality. The bake button is not stored.

| Panel field | What to do with it |
|---|---|
| **Partial Bake For Current Level** | Button. Not stored on the prefab. |
| **RayCast Quality** | `Normal` or `High`. The prefab stores `Normal`. |

## What it does

The probe is the entity. **RayCast Quality** selects the bake quality. Interiors opened below raise it to `High`.

## Where it lives

Native script. Prefab `partial_gi_baker` in `Modules\Native\Prefabs\editor_helpers_&_tests.xml`.

## Examples

**Helper, normal quality.** `editor_helpers_&_tests.xml`, entity `partial_gi_baker`. **RayCast Quality** `Normal`. Editor-only.

**Keep interior.** `NavalDLC\SceneObj\nord_keep_interior_lv1`. Prefab `partial_gi_baker`, hidden, scaled to about `50 × 43 × 25`. **RayCast Quality** `High`. The editor-only visibility mask is turned off.

**Castle.** `SandBoxCore\SceneObj\khuzait_castle_002`. Prefab `partial_gi_baker`, hidden, scaled to about `32 × 23 × 45`. **RayCast Quality** `High`.
