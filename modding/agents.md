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



## Various

How to get the position of a bone on an agent skeleton?

``` cs
boneFrame = TargetAgent.AgentVisuals.GetGlobalFrame().TransformToParent(TargetAgent.AgentVisuals.GetBoneEntitialFrame(i, false));
```


## Outline agent

``` cs
uint focusedContourColor = new TaleWorlds.Library.Color(1f, 0.84f, 0.35f, 1f).ToUnsignedInteger();
agent.AgentVisuals?.SetContourColor(focusedContourColor, true);

agent.AgentVisuals?.SetContourColor(null); to remove it
```

??? example "Looks like this:"

    ![](https://imgur.com/3XV9smr.png)