# Heroes

[TaleWorlds.CampaignSystem.Hero Class Reference](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_hero.html){target=_blank} [1.2.7](https://apidoc.bannerlord.com/v/1.2.7/class_tale_worlds_1_1_campaign_system_1_1_hero.html)

    Hero.MainHero                           // player
    Hero.OneToOneConversationHero           // Hero you are talking to in the dialog


## Find by StringId

``` cs
Hero hero = Hero.FindFirst((Hero x) => x.StringId == "SOME_LORD_ID");
```

## Skills/Attributes/Focus/Xp

![](/pics/ckk9hK4.png)

### Changes

``` cs
Hero.HeroDeveloper.SetInitialSkillLevel(DefaultSkills.OneHanded, 265);
Hero.MainHero.HeroDeveloper.AddAttribute(DefaultCharacterAttributes.Vigor, 1, false);
Hero.MainHero.HeroDeveloper.AddFocus(DefaultSkills.TwoHanded, 1, false);
Hero.MainHero.HeroDeveloper.ChangeSkillLevel(DefaultSkills.OneHanded, 10, true);
Hero.MainHero.HeroDeveloper.AddSkillXp(DefaultSkills.OneHanded, 100, true, true);

Hero.MainHero.AddSkillXp(DefaultSkills.Charm, MBRandom.RandomFloatRanged(10, 50)); // no learning rate applied
```

??? info "How to reduce skill:"
    HeroDeveloper.ChangeSkillLevel does not accept negative values.

    ```cs
    int currentSkill = hero.GetSkillValue(DefaultSkills.Athletics);
    if (currentSkill > 0) hero.SetSkillValue(DefaultSkills.Athletics, currentSkill - 1);
    ```

### Get

``` cs
int skillXP = hero.HeroDeveloper.GetSkillXpProgress(DefaultSkills.Athletics);  // skill XP
int skillValue = hero.GetSkillValue(DefaultSkills.Roguery);    // skill level
int intelligence = hero.GetAttributeValue(DefaultCharacterAttributes.Intelligence);  // skill attribute
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


## Renown

[Clan.Renown](/modding/clans/#renown)

    GainRenownAction.Apply(Hero.MainHero, MBRandom.RandomFloatRanged(1f, 3f), false);

## Traits

[TaleWorlds.CampaignSystem.CharacterDevelopment.DefaultTraits](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_campaign_system_1_1_character_development_1_1_default_traits.html)

``` cs
int Hero.GetTraitLevel(TraitObject trait)
void Hero.SetTraitLevel(TraitObject trait, int value)
void Hero.ClearTraits()

if (hero.GetTraitLevel(DefaultTraits.Honor) < 0) ...

```


??? bug "private (why TW? WHY??) TraitLevelingHelper.AddPlayerTraitXPAndLogEntry"

    Used in game like this:

    `TraitLevelingHelper.AddPlayerTraitXPAndLogEntry(DefaultTraits.Mercy, -30, ActionNotes.VillageRaid, null);`

    But because it's private, we can't use it directly without harmony.

    We can write a duplicate:
    ``` cs
        // duplicate to private TraitLevelingHelper AddPlayerTraitXPAndLogEntry/AddTraitXp methods
        public static void AddPlayerTraitXPAndLogEntry(TraitObject trait, int xpValue, ActionNotes context, Hero referenceHero)
        {
            int traitLevel = Hero.MainHero.GetTraitLevel(trait);

            // AddTraitXp logic
            int currentXp = Campaign.Current.PlayerTraitDeveloper.GetPropertyValue(trait);
            int newXpAmount = xpValue + currentXp;
            int newLevel;
            int newXpValue;
            Campaign.Current.Models.CharacterDevelopmentModel.GetTraitLevelForTraitXp(Hero.MainHero, trait, newXpAmount, out newLevel, out newXpValue);
            Campaign.Current.PlayerTraitDeveloper.SetPropertyValue(trait, newXpValue);
            if (newLevel != Hero.MainHero.GetTraitLevel(trait))
            {
                Hero.MainHero.SetTraitLevel(trait, newLevel);
            }

            // Log and dispatch
            if (traitLevel != Hero.MainHero.GetTraitLevel(trait))
            {
                CampaignEventDispatcher.Instance.OnPlayerTraitChanged(trait, traitLevel);
            }
            if (MathF.Abs(xpValue) >= 10)
            {
                LogEntry.AddLogEntry(new PlayerReputationChangesLogEntry(trait, referenceHero, context));
            }
        }
    ```


??? abstract "ActionNotes"
    * DefaultNote
    * NoQuarrel
    * CourtshipQuarrel
    * FiefQuarrel
    * ValorStrategyQuarrel
    * ResponsibilityStrategyQuarrel
    * CalculatingStrategyQuarrel
    * VengeanceQuarrel
    * DishonestBusinessQuarrel
    * RuthlessBusinessQuarrel
    * CorruptGangLeaderQuarrel
    * CompetingGangLeaderQuarrel
    * TroublemakerQuarrel
    * ExtortingQuarrel
    * HereticQuarrel
    * LandCheatingQuarrel
    * QuestBetrayal
    * QuestSuccess
    * QuestFailed
    * BattleValor
    * HostileAction
    * PersuadedToDefect
    * PartyHungry
    * PartyTakenCareOf
    * VillageRaid
    * SacrificedTroops
    * NPCFreed
    * SiegeAftermath

Personality traits

* `Mercy` - Mercy represents your general aversion to suffering and your willingness to help strangers or even enemies. `Compassionate` (2), `Merciful` (1), `Cruel` (-1), `Sadistic` (-2).
* `Valor` - Valor represents your reputation for risking your life to win glory or wealth or advance your cause. `Fearless` (2), `Daring` (1), `Cautious` (-1), `Very cautious` (-2)
* `Honor` - Honor represents your reputation for respecting your formal commitments, like keeping your word and obeying the law. `Honorable` (2), `Honest` (1), `Devious` (-1), `Deceitful` (-2)
* `Generosity` - Generosity represents your loyalty to your kin and those who serve you, and your gratitude to those who have done you a favor. `Munificent`(2), `Generous` (1), `Closefisted` (-1), `Tightfisted` (-2)
* `Calculating` - Calculating represents your ability to control your emotions for the sake of your long-term interests. `Celebral` (2), `Calculating`, (1) `Impulsive` (-1), `Hotheaded` (-2)

??? example "Defined in &lt;NPCCharacters> files, like lords.xml:"
    ```xml
    <Traits>
        <Trait id="Mercy" value="-1"/>
        <Trait id="Valor" value="2"/>
        <Trait id="Honor" value="0"/>
        <Trait id="Generosity" value="1"/>
        <Trait id="Calculating" value="2"/>
        <Trait id="Egalitarian" value="-1"/>
        <Trait id="Oligarchic" value="1"/>
        <Trait id="Authoritarian" value="2"/>
    </Traits>
    ```

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

??? abstract "NPC99.: Approx relationship between Lord traits and skills:"

    ![](/pics/JCtw0gl.png)

    ![](/pics/YuKhQw2.png)

    ![](/pics/47ysvNJ.png)

### How to add custom trait

!!! quote "Kemo III:"

```cs
public class CustomTraits
{
  private static CustomTraits Current;
  private TraitObject _newTrait;
  public static TraitObject NewTrait
  {
      get
      {
          return Current._newTrait;
      }
  }
  public CustomTraits()
  {   
  Current= this;
  _newTrait = Game.Current.ObjectManager.RegisterPresumedObject(new TraitObject("TraitStringId"));
  _newTrait.Initialize(new TextObject("{=!}Trait Name"), new TextObject("{=!}Trait description."), false, -2, 2);
  }
}
```

You'll need to instantiate this class when loading/starting a campaign. I do it in SubModule `InitializeGameStarter`.

For the new trait to appear in the character screen you'll need to add this harmony patch:

```cs
[HarmonyPatch(typeof(CampaignUIHelper), nameof(CampaignUIHelper.GetHeroTraits))]
public class GetHeroTraitsPatch
{
    static void Postfix(ref IEnumerable<TraitObject> __result)
    {
        __result.AddItem(CustomTraits.NewTrait);
    }
}
```
You also need strings for the trait levels. An xml like this:

``` xml
<?xml version="1.0" encoding="utf-8"?>
<strings>
  <string id="str_trait.TraitStringId" text="Trait Name" />
  <string id="str_trait_name_TraitStringId.0" text="New Trait Lvl -2"/>
  <string id="str_trait_name_TraitStringId.1" text="New Trait Lvl -1"/>
  <string id="str_trait_name_TraitStringId.3" text="New Trait Lvl +1"/>
  <string id="str_trait_name_TraitStringId.4" text="New Trat Lvl +2"/>
</strings>
```
You need to create sprites with that same naming scheme as well. "TraitStringId_-2", "TraitStringId.-1", etc...

!!! warning "Kemo III: Though, I just realized parts of this might be outdated and don't work any more."


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


    <center>![](/pics/LggRpKJ.png)</center>

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


## [Equipment](/modding/equipment)

``` cs
Equipment equipment = Hero.MainHero.BattleEquipment;
Equipment equipment = Hero.MainHero.CivilianEquipment;
```

## Relations

### Get relation

Use this method to get the same relation that is visible in the Encyclopedia:

``` cs
int relation = Hero.MainHero.GetRelation(hero);
int relation = notable.GetRelation(clan.Leader);
```

This returns 'raw` relation not affected by the heroe's traits:

``` cs
int CharacterRelationManager.GetHeroRelation(Hero hero1, Hero hero2)
```


### Set relation

``` cs
CharacterRelationManager.SetHeroRelation(Hero.MainHero, otherHero, -25);
void CharacterRelationManager.SetHeroRelation(Hero hero1, Hero hero2, int value)

// use this one
ChangeRelationAction.ApplyRelationChangeBetweenHeroes(notable, clan.Leader, -20, true);
```

### Are we at war?

``` cs
if (hero?.Clan?.MapFaction?.IsAtWarWith(Hero.MainHero.MapFaction) == true) { // Your code here }
```

## Culture

Hero.Culture

Check: if (hero.Culture.StringId == "sturgia") ..


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

## Age

    hero.Age

Change:
```cs
hero.SetBirthDay(CampaignTime.Now - CampaignTime.Years(30));
```

## Voice

Can be set in the lords.xml file:

``` xml
<NPCCharacter id="lord_lit_1_6" name="Parbus" voice="softspoken" age="52"
```

Voice types:

- curt
- earnest
- ironic
- softspoken

Also depends on the culture.

## Crime Rating


``` cs
Kingdom.MainHeroCrimeRating

public float MainHeroCrimeRating { get; set; }
```


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


## Hero XML

Add an entry into heroes.xml

``` xml
<Hero id="lord_lit_1_1" faction="Faction.clan_lit_1" spouse="Hero.lord_lit_1_2" father="Hero.lit_1_0" mother="Hero.lit_1_00" alive="false" text="Encyclopedia text for the hero" />
```

- id - heroe's ID by which it will be referenced in other files
- faction - clan, in a format: Faction.CLAN_ID
- spouse - Hero.SPOUSE_HERO_ID
- father - Hero.FATHER_HERO_ID
- mother - Hero.MOTHER_HERO_ID
- alive - default true, set to false for dead hero
- text - Encyclopedia text

??? question "How to properly make a lord dead in XML?"
    alive="false" makes him dead, but in description it writes: HERO_NAME is  member of ...


## Lord XML

Add an entry into [lords.xml](/modding/npc_character/)

``` xml

    <NPCCharacter ...


    <!-- Traits -->
    <Traits>
        <Trait id="Mercy" value="-1"/>
        <Trait id="Valor" value="2"/>
        <Trait id="Honor" value="0"/>
        <Trait id="Generosity" value="1"/>
        <Trait id="Calculating" value="2"/>
        <Trait id="Egalitarian" value="-1"/>
        <Trait id="Oligarchic" value="1"/>
        <Trait id="Authoritarian" value="2"/>
    </Traits>
```

## Player start

    SandboxCharacterCreationContent

How to change starting gear info [here](https://discord.com/channels/411286129317249035/411286129317249038/1282512554656272459).

## Clothes for dead heroes

Set clothes for `neutral` culture. A lot of dead heroes will use this:

??? abstract "sandbox_equipment_sets.xml"
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <EquipmentRosters>
    </EquipmentRosters>
    ```

??? abstract "sandboxcore_equipment_sets.xslt"
    ```xml
    <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
        <xsl:output omit-xml-declaration="yes"/>
        <xsl:template match="@*|node()">
            <xsl:copy>
                <xsl:apply-templates select="@*|node()"/>
            </xsl:copy>
        </xsl:template>

        <!-- dead lords gear -->
        <xsl:template match="EquipmentRoster[@id='default_battle_equipment_roster_neutral']/EquipmentSet">
            <EquipmentSet>
                <equipment slot="Body" id="Item.tied_cloth_tunic"/>
                <equipment slot="Leg" id="Item.strapped_shoes"/>
            </EquipmentSet>
        </xsl:template>

        <xsl:template match="EquipmentRoster[@id='default_civilian_equipment_roster_neutral']/EquipmentSet">
            <EquipmentSet>
                <equipment slot="Body" id="Item.tied_cloth_tunic"/>
                <equipment slot="Leg" id="Item.strapped_shoes"/>
            </EquipmentSet>
        </xsl:template>
    </xsl:stylesheet>
    ```


Remaining lords will use equipment that set like this (not sure what's the difference):

spcultures.xml
``` xml
default_battle_equipment_roster="EquipmentRoster.default_battle_equipment_roster_neutral"
default_civilian_equipment_roster="EquipmentRoster.default_civilian_equipment_roster_neutral">
```

In the code it's set as:
```cs
class Campaign

private void InitializeDefaultEquipments()
{
    this.DeadBattleEquipment = Game.Current.ObjectManager.GetObject<MBEquipmentRoster>("default_battle_equipment_roster_neutral").DefaultEquipment;
    this.DeadCivilianEquipment = Game.Current.ObjectManager.GetObject<MBEquipmentRoster>("default_civilian_equipment_roster_neutral").DefaultEquipment;
}
```