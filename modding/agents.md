# Agents

[TaleWorlds.MountAndBlade.Agent Class Reference](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_mount_and_blade_1_1_agent.html){target=_blank}

Agent - moving entity in the Mission: NPC, Player, horse, etc.

* Player agent: Agent.Main
* All Mission agents: Mission.Current.Agents
* Active enemy agents: Mission.Current.PlayerEnemyTeam.ActiveAgents


!!! quote "hunharibo:"
    Agent and Character(Object) both exist and mean different things. <br><br>
    Agent is a "pawn" that moves in a mission (Mission = any scene that is different from the world map, when you walk around town in civilian mode, field battle, siege, arenas, bandit hideouts etc.)<br><br>
    Whereas character is an object container describing an NPC (civilian or troop) and their characteristics and equipment. Agents get spawned partially based on data from a character.

## Agent's Hero

``` cs
if (agent.IsHuman && agent.Character != null && agent.Character.IsHero) {
    Hero? hero = (agent.Character as CharacterObject)?.HeroObject;
}
```

## Public methods

``` cs
.IsMainAgent
.IsPlayerTroop
.IsHero

.IsActive()
.IsRetreating
.IsFadingOut
.IsSliding
.IsSitting
.IsReleasingChainAttack

.IsOnLand

.IsEnemyOf
.IsFriendOf

.IsHuman
.IsAIControlled
.IsPlayerControlled
.IsRunningAway
.IsFemale
.HasWeapon
.HasMount

.IsMount
.RiderAgent // if agent is a horse - gives the rider
.GetSteppedEntity  // returns the game entity agent is standing on

.IsDoingPassiveAttack // whether the agent is in a couched lance or braced spear state

.SpawnEquipment - the equipment the agent spawned with
.Equipment - agent's current equipment

```



## AgentState

agent.State

``` cs
* None
* Active
* Routed
* Unconcious
* Killed
* Deleted
```


## Various actions

``` cs
DropItem(EquipmentIndex itemIndex, WeaponClass pickedUpItemType = WeaponClass.Undefined)
EquipItemsFromSpawnEquipment(bool neededBatchedItems)
EquipWeaponToExtraSlotAndWield(ref MissionWeapon weapon)
RemoveEquippedWeapon(EquipmentIndex slotIndex)
Die(Blow b, Agent.KillInfo overrideKillInfo = Agent.KillInfo.Invalid)
MakeDead(bool isKilled, ActionIndexValueCache actionIndex)
Mount(Agent mountAgent)
SetCrouchMode(bool set)
SetAlwaysAttackInMelee(bool attack)
bool CheckSkillForMounting(Agent mountAgent) - can mount that mountAgent?
```

## [MissionEquipment](/modding/equipment/#missionequipment)

``` cs
public MissionEquipment Equipment { get; private set; }

Agent.DropItem(EquipmentIndex itemIndex, WeaponClass pickedUpItemType = WeaponClass.Undefined)
EquipItemsFromSpawnEquipment(bool neededBatchedItems)
EquipWeaponToExtraSlotAndWield(ref MissionWeapon weapon)
RemoveEquippedWeapon(EquipmentIndex slotIndex)

```

## Position

agent.Position - position on the map in Vec3

agent.Position.AsVec2 - position on the map in Vec2

## Various

How to get the position of a bone on an agent skeleton?

``` cs
boneFrame = TargetAgent.AgentVisuals.GetGlobalFrame().TransformToParent(TargetAgent.AgentVisuals.GetBoneEntitialFrame(i, false));
```

## Get nearby enemy agents

``` cs
MBList<Agent> nearbyEnemyAgents = Mission.GetNearbyEnemyAgents(Agent.Main.Position.AsVec2, 20f, Mission.PlayerTeam, new MBList<Agent>());
```

## Outline agent

``` cs
uint focusedContourColor = new TaleWorlds.Library.Color(1f, 0.84f, 0.35f, 1f).ToUnsignedInteger();
agent.AgentVisuals?.SetContourColor(focusedContourColor, true);

agent.AgentVisuals?.SetContourColor(null); to remove it
```

??? example "Looks like this:"

    ![](/pics/3XV9smr.png)


## BoneBodyPartType

``` cs
public enum BoneBodyPartType : sbyte
{
    None = -1,
    Head,
    Neck,
    Chest,
    Abdomen,
    ShoulderLeft,
    ShoulderRight,
    ArmLeft,
    ArmRight,
    Legs,
    NumOfBodyPartTypes,
    CriticalBodyPartsBegin = 0,
    CriticalBodyPartsEnd = 6
}
```

