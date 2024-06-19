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


## Scouting

    mobileParty.EffectiveScout


## Add hero to a party

    AddCompanionAction.Apply()


