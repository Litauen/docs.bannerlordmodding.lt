# Gold

``` cs
Hero.Gold                               // heroe's gold
Hero.MainHero.Gold                      // player's gold
Settlement.SetttlementComponent.Gold    // settlement's gold
```

## Change gold with notification + sound

``` cs
GiveGoldAction.ApplyBetweenCharacters(null, Hero.MainHero, -100, false);
```

## Change gold without notification/sound

``` cs
workshop.ChangeGold(50);
Hero.MainHero.ChangeHeroGold(-price);
Settlement.SettlementComponent.ChangeGold(goldAmount);
```


## GiveGoldAction methods

``` cs
ApplyForQuestBetweenCharacters(Hero giverHero, Hero recipientHero, int amount, bool disableNotification = false)
ApplyBetweenCharacters(Hero giverHero, Hero recipientHero, int amount, bool disableNotification = false)
ApplyForCharacterToSettlement(Hero giverHero, Settlement settlement, int amount, bool disableNotification = false)
ApplyForSettlementToCharacter(Settlement giverSettlement, Hero recipientHero, int amount, bool disableNotification = false)
ApplyForSettlementToParty(Settlement giverSettlement, PartyBase recipientParty, int amount, bool disableNotification = false)
ApplyForPartyToSettlement(PartyBase giverParty, Settlement settlement, int amount, bool disableNotification = false)
ApplyForPartyToCharacter(PartyBase giverParty, Hero recipientHero, int amount, bool disableNotification = false)
ApplyForCharacterToParty(Hero giverHero, PartyBase receipentParty, int amount, bool disableNotification = false)
ApplyForPartyToParty(PartyBase giverParty, PartyBase receipentParty, int amount, bool disableNotification = false)
```

