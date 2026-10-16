# HandMorphTest

!!! info "v1.4.8"

An editor preview that spawns one test character on the entity and plays a hand-morph pose. It has no panel fields.

Find placed ones with `s:HandMorphTest`. See [Filter](/editor/editor/#filter).

## Where you put it

Add it to an empty entity in an editor scene. The character is placed on that entity's frame, with the frame flattened so it stands upright. No official scene or prefab contains it.

## What you set

This script has no panel fields.

## What it does

After the editor game data finishes loading, it spawns `facgen_template_test_char_0` with morph animations on, age `20`, and white cloth colors. The body plays `act_visual_test_morph_animation` and the hands are set to `act_defend_up_fist_active`. The visual follows the entity every editor tick. Removing the script clears the visual.

## Where it lives

There is no prefab and no scene. The character id is `facgen_template_test_char_0`.

## Examples

No scene or prefab contains `HandMorphTest`. The behavior above is what the script does when you add it in the editor.
