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

## AIBehaviorFlags

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

## MainPartyCurrentAction

    Idle
    GatherAroundHero
    GatherAroundSettlement
    GoToSettlement
    RaidSettlement
    BesiegeSettlement
    PatrolAroundSettlement
    DefendingSettlement

