# Parties

* [API 1.2.7](https://apidoc.bannerlord.com/v/1.2.7/class_tale_worlds_1_1_campaign_system_1_1_party_1_1_mobile_party.html)

## PartyBase

``` cs
.ItemRoster
.MemberRoster
.PrisonRoster

.IsSettlement
.IsMobileParty

.Settlement
.MobileParty
```


## Settlement

``` cs
PartyBase Settlement.Party
```

## MobileParty

Our party:

``` cs
MobileParty.MainParty
```

PartyBase MobileParty.Party

``` cs
.ItemRoster
.MemberRoster
```

## Count members

TroopRoster

``` cs
MobileParty.MainParty.MemberRoster.TotalManCount

.TotalRegulars
.TotalWoundedRegulars
.TotalWoundedHeroes
.TotalHeroes
.TotalWounded
.TotalManCount
.TotalHealthyCount
.TotalHealthyCount
```

## Morale

Get:

    MobileParty.MainParty.Morale
    MobileParty.MainParty.RecentEventsMorale

Set:

    MobileParty.MainParty.RecentEventsMorale += 10;


## Get party Companions

``` cs
for (int i = 0; i < MobileParty.MainParty.MemberRoster.Count; i++)
{
    CharacterObject characterAtIndex = Hero.MainHero.PartyBelongedTo.MemberRoster.GetCharacterAtIndex(i);
    if (characterAtIndex.HeroObject != null && characterAtIndex.HeroObject != Hero.MainHero)
    {
        Hero hero = characterAtIndex.HeroObject;
        // do smthg with the hero (companion)
    }
}
```

## Get effective party role heroes

    party.EffectiveEngineer
    party.EffectiveQuartermaster
    party.EffectiveScout
    party.EffectiveSurgeon


## Add hero to a party

    AddCompanionAction.Apply()


## Destroy

    DestroyPartyAction

[Don't use](https://discord.com/channels/411286129317249035/677511186295685150/1263112214873772043) MobileParty.RemoveParty()


## Main party on game start

Add 50 grain on start:

``` cs
[HarmonyPatch(typeof(Campaign), "InitializeMainParty")]
public class CampaignPatch
{
    [HarmonyPostfix]
    private static void CampaignPostfix(Campaign __instance)
    {
        __instance.MainParty.ItemRoster.AddToCounts(DefaultItems.Grain, 50);
    }
}
```