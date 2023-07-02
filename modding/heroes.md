# Heroes

[TaleWorlds.CampaignSystem.Hero Class Reference](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_hero.html){target=_blank}

    Hero.MainHero

## Skills/Attributes/Focus/Xp

![](https://i.imgur.com/ckk9hK4.png)

### Changes

``` cs
Hero.HeroDeveloper.SetInitialSkillLevel(DefaultSkills.OneHanded, 265);
Hero.MainHero.HeroDeveloper.AddAttribute(DefaultCharacterAttributes.Vigor, 1, false);
Hero.MainHero.HeroDeveloper.AddFocus(DefaultSkills.TwoHanded, 1, false);
Hero.MainHero.HeroDeveloper.ChangeSkillLevel(DefaultSkills.OneHanded, 10, true);
Hero.MainHero.HeroDeveloper.AddSkillXp(DefaultSkills.OneHanded, 100, true, true);
```

### Get

``` cs
int skillValue = Hero.MainHero.GetSkillValue(DefaultSkills.Roguery);
int intelligence = Hero.MainHero.GetAttributeValue(DefaultCharacterAttributes.Intelligence);
```

??? info "DefaultCharacterAttributes"
    * Vigor
    * Control
    * Endurance
    * Cunning
    * Social
    * Intelligence


??? info "DefaultSkills"
    * OneHanded
    * TwoHanded
    * Polearm
    * Bow
    * Crossbow
    * Throwing
    * Riding
    * Athletics
    * Crafting
    * Tactics
    * Scouting
    * Roguery
    * Charm
    * Leadership
    * Trade
    * Steward
    * Medicine
    * Engineering


## Traits

[TaleWorlds.CampaignSystem.CharacterDevelopment.DefaultTraits](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_character_development_1_1_default_traits.html)

``` cs
int Hero.GetTraitLevel(TraitObject trait)
void Hero.SetTraitLevel(TraitObject trait, int value)
void Hero.ClearTraits()
```

Personality traits

* Mercy - Mercy represents your general aversion to suffering and your willingness to help strangers or even enemies. Cruel tag if Mercy < 0.
* Valor - Valor represents your reputation for risking your life to win glory or wealth or advance your cause.
* Honor - Honor represents your reputation for respecting your formal commitments, like keeping your word and obeying the law. Devious tag if Honor < 0.
* Generosity - Generosity represents your loyalty to your kin and those who serve you, and your gratitude to those who have done you a favor.
* Calculating - Calculating represents your ability to control your emotions for the sake of your long-term interests.


??? abstract "Other traits"

    Political:

    * Egalitarian
    * Oligarchic
    * Authoritarian

    Voice:

    * PersonaCurt
    * PersonaEarnest
    * PersonaIronic
    * PersonaSoftspoken

    Profession?

    * Surgery
    * Siegecraft
    * Blacksmith
    * Fighter
    * Commander
    * Politician
    * Manager
    * Trader
    * Thug
    * Thief
    * Gambler
    * Smuggler

    Skills:

    * ScoutSkills
    * RogueSkills
    * SergeantCommandSkills
    * KnightFightingSkills
    * CavalryFightingSkills
    * HorseArcherFightingSkills
    * HopliteFightingSkills
    * ArcherFIghtingSkills
    * CrossbowmanStyle
    * PeltastFightingSkills
    * HuscarlFightingSkills
    * BossFightingSkills

    Equipment:

    * WandererEquipment
    * GentryEquipment

    Body build:

    * MuscularBuild
    * HeavyBuild
    * LightBuild
    * OutOfShapeBuild

    Hair:

    * RomanHair
    * FrankishHair
    * CelticHair
    * RusHair
    * ArabianHair
    * SteppeHair



## Health

``` cs
bool IsHealthFull()
void Heal(int healAmount, bool addXp=false)
void MakeWounded(Hero killerHero=null, KillCharacterAction.KillCharacterActionDetail deathMarkDetail=KillCharacterAction.KillCharacterActionDetail.None)
int  MaxHitPoints [get]
int  HitPoints [get, set]
```

!!! question "How to increase max health (MaxHitPoints) of the hero?"


## Power

    float hero.Power
    void  hero.AddPower (float value)


??? abstract "Defines how powerfull a notable is (Regular [0..99], Influential [100..199], Powerful [200+])"


    <center>![](https://imgur.com/LggRpKJ.png)</center>

        private DefaultNotablePowerModel.NotablePowerRank[] NotablePowerRanks = new DefaultNotablePowerModel.NotablePowerRank[]
        {
            new DefaultNotablePowerModel.NotablePowerRank(new TextObject("{=aTeuX4L0}Regular", null), 0, 0.05f),
            new DefaultNotablePowerModel.NotablePowerRank(new TextObject("{=nTETQEmy}Influential", null), 100, 0.1f),
            new DefaultNotablePowerModel.NotablePowerRank(new TextObject("{=UCpyo9hw}Powerful", null), 200, 0.15f)
        };



A lof of stuff done in NotablePowerModel ([DefaultNotablePowerModel Class](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_game_components_1_1_default_notable_power_model.html){target=_blank}), example:

    int Campaign.Current.Models.NotablePowerModel.RegularNotableMaxPowerLevel

## Occupation

hero.SetNewOccupation(Occupation.Special); // spawns in the keep
hero.SetNewOccupation(Occupation.Wanderer); // spawns in the tavern, engine relocates to random town once a week

??? info "Occupation list"

    * NotAssigned
    * Tavernkeeper
    * Mercenary
    * Lord
    * GoodsTrader
    * ArenaMaster
    * Villager
    * Soldier
    * Townsfolk
    * RansomBroker
    * Weaponsmith
    * Armorer
    * HorseTrader
    * TavernWench
    * TavernGameHost
    * Bandit
    * Wanderer
    * Artisan
    * Merchant
    * Preacher
    * Headman
    * GangLeader
    * RuralNotable
    * PrisonGuard
    * Guard
    * ShopWorker
    * Musician
    * Gangster
    * Blacksmith
    * BannerBearer
    * CaravanGuard
    * Special

    Check for occupation:

    * hero.IsGangLeader
    * hero.IsMerchant
    * hero.IsArtisan
    * hero.IsLord
    * hero.IsPreacher
    * hero.IsWanderer
    * hero.IsRuralNotable
    * hero.IsUrbanNotable
    * hero.IsHeadman
    * hero.IsMinorFactionHero
    * hero.IsNotable


## Relations

``` cs
CharacterRelationManager.SetHeroRelation(Hero.MainHero, otherHero, -25);
void CharacterRelationManager.SetHeroRelation(Hero hero1, Hero hero2, int value)
int CharacterRelationManager.GetHeroRelation(Hero hero1, Hero hero2)
int relation = notable.GetRelation(clan.Leader);
ChangeRelationAction.ApplyRelationChangeBetweenHeroes(notable, clan.Leader, -20, true);
```


## States

``` cs
Hero.HeroState
Hero.ChangeState(Hero.CharacterStates.Active);
```

??? info "CharacterStates"
    * NotSpawned
    * Active
    * Fugitive
    * Prisoner
    * Released
    * Dead
    * Disabled
    * Traveling


## Attributes

* IsKnown - we checked him in Encyclopedia, met him
* HasMet - we actualy met him (participated in dialog)
* LastMeetingTimeWithPlayer - last time we met him
* Hero.EncyclopediaText = new TextObject("Alive and kicking!"); - Encyclopedia text

## Kill Character

``` cs
KillCharacterAction.ApplyByDeathMarkForced(vendor, true);
```

??? info "Other ways to kill"
    * ApplyByOldAge     // did not work in console cmd test
    * ApplyByWounds
    * ApplyByBattle     // did not work in console cmd test
    * ApplyByMurder
    * ApplyInLabor
    * ApplyByExecution
    * ApplyByRemove     // worked
    * ApplyByDeathMark
    * ApplyByDeathMarkForced
    * ApplyByPlayerIllness

Based on how the hero was killed, Hero.DeathMark is created

??? info "Hero.DeathMark values"
    ```cs
    public enum KillCharacterActionDetail
    {
        None,
        Murdered,
        DiedInLabor,
        DiedOfOldAge,
        DiedInBattle,
        WoundedInBattle,
        Executed,
        Lost
    }

    An obituary in the Encyclopedia depends on it (he died of natural causes/was killed/disappeared/etc):

    victim.EncyclopediaText = KillCharacterAction.CreateObituary(victim, actionDetail);
    ```

Also Hero.DeathMarkKillerHero points to the killer Hero if applicable (public void AddDeathMark).



## Not-wounded party heroes list

``` cs
List<Hero> heroList = (from characterObject in Hero.MainHero.PartyBelongedTo.MemberRoster.GetTroopRoster()
   where characterObject.Character.HeroObject != null && !characterObject.Character.HeroObject.IsWounded
   select characterObject.Character.HeroObject
   ).ToList<Hero>();
```


## Special Items

    public List<ItemObject> SpecialItems


## Conversation Tags

[TaleWorlds.CampaignSystem.Conversation.Tags](https://apidoc.bannerlord.com/v/1.1.0/namespace_tale_worlds_1_1_campaign_system_1_1_conversation_1_1_tags.html)

??? abstract "List"

    * AlliedLordTag
    * AmoralTag
    * AnyNotableTypeTag
    * ArtisanNotableTypeTag
    * AseraiTag
    * AttackingTag
    * AttractedToPlayerTag
    * BattanianTag
    * CalculatingTag
    * CautiousTag
    * ChivalrousTag
    * CombatantTag
    * ConversationTag
    * ConversationTagHelper
    * CruelTag
    * CurrentConversationIsFirst
    * DefaultTag
    * DeviousTag
    * DrinkingInTavernTag
    * EmpireTag
    * EngagedToPlayerTag
    * FirstMeetingTag
    * FriendlyRelationshipTag
    * GangLeaderNotableTypeTag
    * GenerosityTag
    * HeadmanNotableTypeTag
    * HighRegisterTag
    * HonorTag
    * HostileRelationshipTag
    * ImpoliteTag
    * ImpulsiveTag
    * InHomeSettlementTag
    * KhuzaitTag
    * LowRegisterTag
    * MerchantNotableTypeTag
    * MercyTag
    * MetBeforeTag
    * NoConflictTag
    * NonCombatantTag
    * NonviolentProfessionTag
    * NpcIsFemaleTag
    * NpcIsLiegeTag
    * NpcIsMaleTag
    * NpcIsNobleTag
    * OldTag
    * OnTheRoadTag
    * OutlawSympathyTag
    * PersonaCurtTag
    * PersonaEarnestTag
    * PersonaIronicTag
    * PersonaSoftspokenTag
    * PlayerBesiegingTag
    * PlayerIsAffiliatedTag
    * PlayerIsBrotherTag
    * PlayerIsDaughterTag
    * PlayerIsEnemyTag
    * PlayerIsFamousTag
    * PlayerIsFatherTag
    * PlayerIsFemaleTag
    * PlayerIsKinTag
    * PlayerIsKnownButNotFamousTag
    * PlayerIsLiegeTag
    * PlayerIsMaleTag
    * PlayerIsMotherTag
    * PlayerIsNobleTag
    * PlayerIsRulerTag
    * PlayerIsSisterTag
    * PlayerIsSonTag
    * PlayerIsSpouseTag
    * PreacherNotableTypeTag
    * RogueSkillsTag
    * RomanticallyInvolvedTag
    * SexistTag
    * SturgianTag
    * TribalRegisterTag
    * UncharitableTag
    * UnderCommandTag
    * UngratefulTag
    * ValorTag
    * VlandianTag
    * VoiceGroupPersonaCurtLowerTag
    * VoiceGroupPersonaCurtTribalTag
    * VoiceGroupPersonaCurtUpperTag
    * VoiceGroupPersonaEarnestLowerTag
    * VoiceGroupPersonaEarnestTribalTag
    * VoiceGroupPersonaEarnestUpperTag
    * VoiceGroupPersonaIronicLowerTag
    * VoiceGroupPersonaIronicTribalTag
    * VoiceGroupPersonaIronicUpperTag
    * VoiceGroupPersonaSoftspokenLowerTag
    * VoiceGroupPersonaSoftspokenTribalTag
    * VoiceGroupPersonaSoftspokenUpperTag
    * WandererTag
    * WaryTag
