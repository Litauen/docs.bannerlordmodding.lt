# AgentBindsMachine

!!! info "v1.4.8"

A use point on a chain. The player stands on its pilot point, plays the short break-chains action, and the oarsman tied to that chain is released.

Find placed ones in the Scene panel with `s:AgentBindsMachine`. See [Filter](/editor/editor/#filter).

## Where you put it

Add the script to the chain entity. Give that entity one of the tags the captivity mission looks up. Add a child entity tagged `Pilot` with a `StandingPoint`. Vanilla names that child `pilot` and gives it the editor-only man icon.

The script does not choose the oar. The captivity mission calls that link from the entity tag.

## What you set

The script adds no fields of its own. The four rows on the panel come from the usable-machine base. Vanilla leaves every one at the default.

| Panel field | What to do with it |
|---|---|
| **AmmoPickUpTag** | Tag of an ammo standing point. Default `ammopickup`. The chain does not use one. |
| **NavMeshPrefabName** | Navmesh prefab to import for this machine. Vanilla leaves it empty. |
| **WaitStandingPointTag** | Tag of a wait point. Default `Wait`. The chain does not use one. |
| **PilotStandingPointTag** | Tag of the standing point the player uses. Default `Pilot`. The child `pilot` entity carries this tag. |

The entity tag is what distinguishes the chains:

| Tag | Who is freed |
|---|---|
| `agentbind_ally` | The ally on the oar tagged `target_ally` |
| `agentbind_neutral_1` | Crew on the oar tagged `spawnpoint_neutral_npc_1` |
| `agentbind_neutral_2` | Crew on the oar tagged `spawnpoint_neutral_npc_2` |
| `agentbind_neutral_3` | Crew on the oar tagged `spawnpoint_neutral_npc_3` |

A matching `agentbind_*_broken` chain sits beside each one, starts invisible, and has no script.

## What the player gets

The use prompt is **Chains**, action **Use**. The user plays `act_cutscene_break_chains_short`. When that action finishes, the user and the oarsman both let go. While the oar still has someone on it, the objective marker reads **Unchain**. The objective is **Escape Captivity**: set all prisoners on the ship free.

## Where it lives

Managed script on `UsableMachine`. The only vanilla placement is the NavalDLC prefab `storyline_mesh_holder`, packed in `Modules\NavalDLC\AssetPackages\nested_prefabs_packed.xml`. The captivity mission that reads the tags is `NavalCaptivityBattle`. It spawns the hull `ship_knarr_storyline`.

Saved on each chain, from that prefab:

``` xml
<script name="AgentBindsMachine">
    <variables>
        <variable name="AmmoPickUpTag" value="ammopickup"/>
        <variable name="NavMeshPrefabName" value=""/>
        <variable name="WaitStandingPointTag" value="Wait"/>
        <variable name="PilotStandingPointTag" value="Pilot"/>
    </variables>
</script>
```

## Examples

All four are `torture_tool_chain_c` entities on `storyline_mesh_holder`. Each stores the same four defaults and a child `pilot` standing point.

**Ally's chains.** Tag `agentbind_ally`. The mission ties this use point to the oar whose spawn is tagged `target_ally`.

**First crew chains.** Tag `agentbind_neutral_1`. Tied to the oar tagged `spawnpoint_neutral_npc_1`.

**Second crew chains.** Tag `agentbind_neutral_2`. Tied to `spawnpoint_neutral_npc_2`.

**Third crew chains.** Tag `agentbind_neutral_3`. Tied to `spawnpoint_neutral_npc_3`.

No `scene.xscene` under Native, SandBox, or NavalDLC places this script. All 89 NavalDLC scenes were scanned: none contain `AgentBindsMachine`, an `agentbind_` tag, or the prefab `storyline_mesh_holder`.

Eleven of those scenes do drop the chain mesh (`aserai_shipyard`, `battania_shipyard`, `cutscene_saving_sister`, `empire_shipyard`, `khuzait_shipyard`, `naval_storyline_act_3_quest_5`, `nord_dungeon_stealth`, `nord_keep_interior_lv2`, `nord_town_d`, `storyline_shipyard_alley`, `vlandia_shipyard`). A `prefab="torture_tool_chain_c"` there is the Native mesh from `building_details.xml`: the mesh only, no script and no pilot point. The same is true of `torture_tool_chain_a`, `_b`, and `_d`. The four scripted chains above are the vanilla set.
