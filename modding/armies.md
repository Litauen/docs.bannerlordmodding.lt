# Armies

* [API](https://apidoc.bannerlord.com/v/1.2.12/class_tale_worlds_1_1_campaign_system_1_1_army.html)

## Properties

Type            |Name                                |       |   |
----------------|------------------------------------|-------|---|
TextObject      | EncyclopediaLinkWithName           | [get] | |
[AIBehaviorFlags](/modding/armies/#aibehaviorflags) | AIBehavior                         | [get, set] | |
[ArmyTypes](/modding/armies/#armytypes)       | ArmyType                           | [get, set] | |
Hero            | ArmyOwner                          | [get, set] | |
float           | Cohesion                           | [get, set] | |
float           | DailyCohesionChange                | [get] | |
ExplainedNumber | DailyCohesionChangeExplanation     | [get] | |
int             | CohesionThresholdForDispersion     | [get] | |
float           | Morale                             | [get] | |
MobileParty     | LeaderParty                        | [get] | |
int             | LeaderPartyAndAttachedPartiesCount | [get] | |
Kingdom         | Kingdom                            | [get, set] | |
IMapPoint       | AiBehaviorObject                   | [get, set] | |
TextObject      | Name                               | [get] | |
float           | TotalStrength                      | [get] | total strength of all parties in the army |
int             | TotalHealthyMembers                | [get] | |
int             | TotalManCount                      | [get] | |
int             | TotalRegularCount                  | [get] | |


## ArmyTypes

    Besieger
    Raider
    Defender
    Patrolling

## AIBehaviorFlags (1.2.12)

    Unassigned = 0
    PreGathering
    Gathering
    WaitingForArmyMembers
    TravellingToAssignment
    Besieging
    AssaultingTown
    Raiding
    Defending
    Patrolling
    GoToSettlement

## MainPartyCurrentAction (1.2.12)

    Idle
    GatherAroundHero
    GatherAroundSettlement
    GoToSettlement
    RaidSettlement
    BesiegeSettlement
    PatrolAroundSettlement
    DefendingSettlement

## Food

```cs
public static int GetDaysOfFoodLeftForArmy(Army army)
{
    if (army == null || army.Parties == null || army.Parties.Count == 0) return 0;

    double totalDays = 0.0;
    int partyCount = 0;

    foreach (MobileParty mobileParty in army.Parties)
    {
        if (mobileParty.FoodChange < 0)
        {
            float daysUntilNoFood = mobileParty.Food / -mobileParty.FoodChange;
            totalDays += Math.Max(daysUntilNoFood, 0f);
            partyCount++;
        }
    }

    int daysLeft = (int)MathF.Ceiling((float)(totalDays / partyCount));
    return daysLeft;
}
```

## Disband

```cs
DisbandArmyAction.ApplyByReleasedByPlayerAfterBattle(army);
DisbandArmyAction.ApplyByArmyLeaderIsDead(army);
DisbandArmyAction.ApplyByNotEnoughParty(army);
DisbandArmyAction.ApplyByObjectiveFinished(army);
DisbandArmyAction.ApplyByPlayerTakenPrisoner(army);
DisbandArmyAction.ApplyByFoodProblem(army);
DisbandArmyAction.ApplyByCohesionDepleted(army);
DisbandArmyAction.ApplyByNoActiveWar(army);
DisbandArmyAction.ApplyByUnknownReason(army);
DisbandArmyAction.ApplyByLeaderPartyRemoved(army);
```
