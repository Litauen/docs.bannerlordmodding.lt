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

    DestroyPartyAction.Apply()

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

## Party Size

!!! quote "Sly:"

    If anyone one day is looking for info on how the AI adjusts party sizes:

    MakeClanFinancialEvaluation sets party wage caps : >90k clan gold = 10k, ]30k, 90k] = ]600, 1200], <30k = [200, 600]
    a MobileParty has its PaymentLimit set by the above
    CalculateMobilePartyMemberSizeLimit calculates party size limit
    a party has a LimitedPartySize if the ClanFinancialEval sets one (always for  kingdom/minor faction clans)
    this cap is only lower than the normal calculation if finances throttle PaymentLimit
    the cap is roughly PaymentLimit/(average wage/troop in the default culture hero party template)
    if a party exceeds its PaymentLimit, GetNumberOfDeserters calculates a number of deserters based on the (wages paid beyond the limit)/(4*AvgCulturalTemplateWage); a maxmimum of 20 desertions per day is allowed
    PartiesCheckDesertionDueToPartySizeExceedsPaymentRatio checks for troops in excess of party limit or wage limit
    it then attempts to remove the lowest tier units first until it reaches the  #ofDeserters

## Position on the World Map

    party.Position2D.x
    party.Position2D.y
