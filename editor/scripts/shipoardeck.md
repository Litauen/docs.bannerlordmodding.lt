# ShipOarDeck

!!! info "v1.4.8"

The angles every oar on this deck swings through. Oar length is measured from the gate meshes. It does not place the oars.

Find placed ones in the Scene panel with `s:ShipOarDeck`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the deck entity. The loose prefab `oar_deck` in `NavalDLC\Prefabs\ship_machines.xml` has children `right_oar_machines` and `left_oar_machines`. Gate meshes are children tagged `oar_gate_left` and `oar_gate_right`. The longest axis of those meshes is the oar length.

Packed ships put the script on `oar_deck` inside `oars_holder`.

## What you set

The packed deck stores the four angle fields. Degrees in the panel are converted to radians for the oar stroke.

| Panel field | What to do with it |
|---|---|
| **_verticalBaseAngle** | Rest pitch of the oar, in degrees. Code default `15`. The packed deck stores `15`. |
| **_lateralBaseAngle** | Rest yaw of the oar, in degrees. Code default `0`. The packed deck stores `0`. |
| **_verticalRotationAngle** | How far the oar pitches during a stroke, in degrees. Code default `10`. The packed deck stores `10`. |
| **_lateralRotationAngle** | How far the oar sweeps, in degrees. Code default `17.2`. The packed deck stores `17.200`. |

The packed deck also stores `FactorColor`. This class does not declare that field.

The loose `oar_deck` stores a different row list: `VerticalBaseAngle`, `LateralBaseAngle`, `VerticalRotationAngle`, `LateralRotationAngle`, `VerticalNoisePercentage`, `LateralNoisePercentage`, `OarLength`, `LeftOarMachinePrefab`, `RightOarMachinePrefab`, `OarCountOneSide`, `OarSpan`, `OarGateOffsetSide`, `OarGateOffsetBackward`, `OarGateOffsetUp`, `RetractedFrameOffset`, `RetractedFrameAngle`, `UseExistingOarMachineFrames`, `ShowDebugPreview`. Those names are not fields on this class. The four angle numbers there match the code defaults (`15`, `0`, `10`, `17.2`).

## What it does

Each oar machine and unmanned oar on the deck reads these angles. The oar length is the longest bounding-box axis of the gate mesh, or of the `upgrade_slot` child when the gate itself has no mesh.

## Where it lives

Managed script. Loose prefab `oar_deck` in `NavalDLC\Prefabs\ship_machines.xml`. Packed prefab `oars_holder` in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`.

## Examples

No scene file stores this script.

**Packed deck.** Prefab `oars_holder`, entity `oar_deck`. The four angles are `15`, `0`, `10`, `17.200`. One copy stores `FactorColor` `0.106, 0.094, 0.071`. Another stores `1, 1, 1`.

**Loose editor deck.** `NavalDLC\Prefabs\ship_machines.xml`, entity `oar_deck`. Children `right_oar_machines` and `left_oar_machines`. The stored names are the older list above.
