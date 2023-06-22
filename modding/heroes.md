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


## Health

``` cs
bool IsHealthFull()
void Heal(int healAmount, bool addXp=false)
void MakeWounded(Hero killerHero=null, KillCharacterAction.KillCharacterActionDetail deathMarkDetail=KillCharacterAction.KillCharacterActionDetail.None)
int  MaxHitPoints [get]
int  HitPoints [get, set]
```

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
