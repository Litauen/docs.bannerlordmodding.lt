# PopupSceneShipController

!!! info "v1.4.8"

Pushes or anchors one ship in a notification scene. The ship is the entity with the tag you type, not the entity this script sits on.

Find placed ones in the Scene panel with `s:PopupSceneShipController`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to an empty entity in the scene. Tag the ship you want to move. The force is applied at this entity’s center of mass, in the ship’s local axes.

## What you set

The editor shows the private fields and two buttons. The scene stores the three values. The buttons are not saved.

| Panel field | What to do with it |
|---|---|
| **_continousForce** | Local force, multiplied by the ship’s mass. The vanilla scene stores `(0, 45, 0)`. |
| **_isAnchored** | On: the ship’s naval physics drops an anchor and the force is not applied. Default off. |
| **_targetShipEntityTag** | Tag of the ship entity. The vanilla scene stores `funeral_boat`. |
| **StartApplyingForce** | Editor button. Starts the push and remembers the ship frame. |
| **StopApplyingForce** | Editor button. Puts the ship back on that frame and clears its velocity. |

On play, the push is already on. **StopApplyingForce** is what turns it off in the editor.

## What it does

Each physics step, if the ship is not anchored and the push is on, the force is applied at this entity’s center of mass. If **_isAnchored** is on, the ship’s naval physics is anchored instead and the force is skipped.

## Where it lives

Managed script. One scene stores it.

## Examples

**Funeral boat pushed forward.** `NavalDLC\SceneObj\scn_cutscene_main_hero_naval_battle_death`. Entity `ship_scripts`. Force `(0, 45, 0)`, anchor off, tag `funeral_boat`. The tagged entity is `funeral_boat_nested`, mass `600`, gravity off.
