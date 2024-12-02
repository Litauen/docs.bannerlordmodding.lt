# Missions

- [TaleWorlds.MountAndBlade.Mission Class Reference](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_mount_and_blade_1_1_mission.html){target=_blank}
- [MissionViews](/gauntletui/mission_views/)

Where you control your character in 3D environment. Not a global map. Examples: Town, village, siege, battles, hideouts, prison break


    MissionBehaviour (PURE MissionBehavior not present in the game)

    MissionView - subtype of MissionBehaviour - views, icons, scores, etc
        shows some UI to the player
        handles input
        listens to keypress

    MissionLogic - subtype of MissionBehaviour - manages the mission in a some way
        spawning agents
        handling reinformcements
        handling routing



## MissionMode

```cs
if (Mission.Current?.Mode == MissionMode.Battle)...
```

```cs
public enum MissionMode
{
    StartUp,
    Conversation,
    Battle,
    Duel,
    Stealth,
    Barter,
    Deployment,
    Tournament,
    Replay,
    CutScene,
    Benchmark
}
```

## Method run order

```
OnCreated
EarlyStart
AfterStart
    Mission.Current.PlayerEnemyTeam.ActiveAgents == 0 here
OnDeploymentFinished
    In the battle mission when actual battle starts
    Mission.Current.PlayerEnemyTeam.ActiveAgents populated here
OnRenderingStarted
OnEndMission
```

## OnAgentHit

``` cs
void OnAgentHit(Agent affectedAgent, Agent affectorAgent, in MissionWeapon attackerWeapon, in Blow blow, in AttackCollisionData attackCollisionData)

int inflictedDamage = blow.InflictedDamage;

// weapon
WeaponComponentData attackerWeaponComponentData = attackerWeapon.CurrentUsageItem;
attackerWeaponComponentData.WeaponClass

bool blow.IsMissile

bool blow.IsHeadShot

BoneBodyPartType blow.VictimBodyPart

```


## Teams

``` cs
public Mission.TeamCollection Teams { get; private set; }

AttackerTeam
DefenderTeam
AttackerAllyTeam
DefenderAllyTeam
PlayerTeam
PlayerEnemyTeam
PlayerAllyTeam
SpectatorTeam
```


## Find Entity and it's position in scene

TaleWorlds.Engine.Scene.FindEntityWithName or FindEntityWithTag and then you can get the entity Vec3 position from GameEntity.GlobalPosition


## Max scene coordinates

``` cs
Vec2 terrainSize = new Vec2(1f, 1f);

Mission.Scene.GetTerrainData(out Vec2i nodeDimension, out float nodeSize, out _, out _);
terrainSize.x = nodeDimension.X * nodeSize;
terrainSize.y = nodeDimension.Y * nodeSize;
```
