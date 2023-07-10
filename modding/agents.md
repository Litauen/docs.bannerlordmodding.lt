# Agents

[TaleWorlds.MountAndBlade.Agent Class Reference](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_mount_and_blade_1_1_agent.html){target=_blank}

Agent - moving entity in the Mission: NPC, Player, horse, etc.

* Player agent: Agent.Main
* All Mission agents: Mission.Current.Agents
* Active enemy agents: Mission.Current.PlayerEnemyTeam.ActiveAgents

## Agent's Hero

``` cs
if (agent.IsHuman && agent.Character != null && agent.Character.IsHero) {
    Hero? hero = (agent.Character as CharacterObject)?.HeroObject;
}
```

## Properties

``` cs
.IsMainAgent
.IsPlayerTroop
.IsHero
.IsActive()
.IsHuman
.IsMount
.IsAIControlled
.IsPlayerControlled
.IsRunningAway
.IsFemale
.HasMount
```
