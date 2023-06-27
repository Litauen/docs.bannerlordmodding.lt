# Parties

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


## Scouting

    mobileParty.EffectiveScout


## Examples

Player's clan parties count:

    int active = MobileParty.All.Where(party => party.LeaderHero?.Clan == Clan.PlayerClan).Count;