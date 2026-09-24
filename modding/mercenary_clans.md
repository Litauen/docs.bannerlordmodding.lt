# Custom Mercenary Clans

!!! info "v1.4.8"

How to add a mercenary company that spawns lord parties and can be hired by a kingdom.

Mercenary clans are **not bandits** and they are **not created in C#**. The game loads a `<Faction>` with `is_minor_faction="true"`. `Clan.Deserialize` builds the clan. `HeroSpawnCampaignBehavior` turns `minor_faction_character_templates` into lords. Parties come from `default_party_template` the same way other lord parties do.

Hiring is separate. `DiplomaticBartersBehavior` (daily, AI kingdoms) and the lord dialog (player kingdom ruler) call `ChangeKingdomAction.ApplyByJoinFactionAsMercenary`. There is **no contract length** in XML. `ShouldStayInKingdomUntil` is left at zero, so the clan can leave as soon as the leave score is high enough.

`is_clan_type_mercenary="true"` marks the company in data (native Ghilman, Legion of the Betrayed, Skolderbroda, Company of the Golden Boar). The AI hire and leave rolls use `Clan.IsMinorFaction`, so mafia / sect / nomad minors can take the same mercenary contract. The mercenary flag only blocks the noble-style defection roll.

Do **not** set `is_bandit`. That is the hideout path. See [Bandit Clans](/modding/bandit_clans/).

`is_outlaw` is not the hideout flag. On a minor clan it makes `DiplomacyModel.IsAtConstantWar` true against **same-culture kingdoms**, and the AI hire roll refuses constant war. Native mafia such as `beni_zilal` is `is_outlaw` and not `is_bandit`. Leave `is_outlaw` off a company you want hired by its own culture.

Related: [Clans](/modding/clans/), [Cultures](/modding/cultures/), [Parties](/modding/parties/), [Kingdoms](/modding/kingdoms/).

---

## The chain

```
Faction  (is_minor_faction="true", optional is_clan_type_mercenary="true")
  → Culture of a normal people (Culture.empire, …) — not a bandit culture
  → default_party_template  (lord party stacks)
  → minor_faction_character_templates  (NPCCharacter is_template="true", occupation="Lord")
```

No hideouts. No `super_faction` (they start independent). Heroes are not listed in `heroes.xml`; templates spawn them.

---

## 1. Faction XML

Load `Factions` from your module (`SubModule.xml` → `XmlName id="Factions"`). Native file is `SandBox/ModuleData/spclans.xml`. Native mercenaries are the four nodes under the comment `mercenary minor factions (4)`.

```xml
<Factions>
  <Faction
    id="ghilman"
    initial_home_settlement="Settlement.castle_A7"
    banner_key="11.191.154.1536.1536.764.764.1.0.0.401.116.155.488.488.764.764.0.0.0"
    color="FFF1C178"
    color2="FF0B0C11"
    culture="Culture.darshi"
    default_party_template="PartyTemplate.kingdom_hero_party_mercenary_aserai_template"
    settlement_banner_mesh="encounter_flag_f"
    is_minor_faction="true"
    is_clan_type_mercenary="true"
    name="{=ghilmanClanName}Ghilman"
    tier="4"
    text="{=4TiYXBqK}The Ghilman are a band of mercenaries...">
    <minor_faction_character_templates>
      <template id="NPCCharacter.spc_ghilman_leader_0" />
      <template id="NPCCharacter.spc_ghilman_leader_1" />
      <template id="NPCCharacter.spc_ghilman_leader_2" />
      <template id="NPCCharacter.spc_ghilman_leader_3" />
    </minor_faction_character_templates>
  </Faction>
</Factions>
```

Copy that node. Change `id`, `name`, `text`, `culture`, `default_party_template`, `initial_home_settlement`, and the four `<template>` ids. Keep both flags.

| Attribute | Effect |
| --- | --- |
| `id` | `Clan.StringId`. |
| `is_minor_faction="true"` | `Clan.IsMinorFaction`. This is what the daily hire/leave code checks. |
| `is_clan_type_mercenary="true"` | `Clan.IsClanTypeMercenary`. Labels the clan as a mercenary company. Skips the defection roll in `DiplomaticBartersBehavior`. |
| `culture` | A normal culture. Troop culture on the party template can differ (native Ghilman are `Culture.darshi` and field Aserai `ghilman_tier_*` troops). |
| `default_party_template` | Stacks used when a lord party is created. |
| `initial_home_settlement` | `SetInitialHomeSettlement` only. A town, castle, or village. Not a hideout. |
| `tier` | Starting tier. Native mercenaries are `4`. Initial renown comes from `ClanTierModel.CalculateInitialRenown`. |
| `name`, `short_name`, `text` | Encyclopedia. `text` becomes `EncyclopediaText`. |
| `banner_key`, `color`, `color2` | Banner. If `banner_key` is omitted, `Banner.CreateRandomClanBanner` runs. |
| `owner` | Not set on native mercenaries. The first spawned template hero becomes leader. |
| `super_faction` | Do not set. They join a kingdom only through mercenary service. |

Child `<relationship value="-1" kingdom="Kingdom...." />` declares war at load (`Clan.Deserialize`). Native mercenary nodes have none. They start at peace and the AI will not join a kingdom it is already at war with.

---

## 2. Character templates

Native leaders live in `SandBox/ModuleData/spspecialcharacters.xml`. They are templates, not `heroes.xml` entries (the `<Hero>` line under Ghilman is commented out).

```xml
<NPCCharacter
  id="spc_ghilman_leader_0"
  name="{=IgOTjF4i}of the Ghilman"
  voice="earnest"
  is_template="true"
  default_group="Infantry"
  is_hero="false"
  culture="Culture.aserai"
  skill_template="SkillSet.spc_ghilman_leader_0"
  occupation="Lord">
  <face>
    <face_key_template value="BodyProperty.fighter_ghilman" />
  </face>
  <Traits>
    <Trait id="Valor" value="1" />
    <Trait id="Generosity" value="1" />
    <Trait id="Commander" value="14" />
    <Trait id="BalancedFightingSkills" value="3" />
    <Trait id="KnightFightingSkills" value="5" />
  </Traits>
</NPCCharacter>
```

`Equipments` on that character point at `EquipmentSet` ids in `sandbox_equipment_sets.xml` (`spc_ghilman_leader_0`, plus a civilian set). Copy that block with the traits; do not leave `Equipments` empty if you want them armed.

`HeroSpawnCampaignBehavior.SpawnMinorFactionHeroes`:

- New game fills up to `MinorFactionHeroLimit` (**4**) from the template list, in order.
- Later days: if alive lords are still under 4, each empty slot has `DailyMinorFactionHeroSpawnChance` (**0.1**) to spawn a random template.
- `HeroCreator.CreateSpecialHero` + `IsMinorFactionHero = true`.
- Empty `minor_faction_character_templates` hits a `FailedAssert` and spawns nobody.

Give four templates if you want the native lord count on day one.

---

## 3. Party templates

Native file: `SandBox/ModuleData/partyTemplates.xml`, comment `party templates for mercenary minor factions (4)`.

```xml
<MBPartyTemplate id="kingdom_hero_party_mercenary_aserai_template">
  <stacks>
    <PartyTemplateStack min_value="4" max_value="4" troop="NPCCharacter.ghilman_tier_3" />
    <PartyTemplateStack min_value="8" max_value="8" troop="NPCCharacter.ghilman_tier_2" />
    <PartyTemplateStack min_value="16" max_value="16" troop="NPCCharacter.ghilman_tier_1" />
  </stacks>
</MBPartyTemplate>
```

Your troops are normal `NPCCharacter` soldiers (upgrade tree, `occupation="Soldier"`). They are not the leader templates.

No `<ship_hulls>` on the land companies. A template with ships makes `Clan.HasNavalNavigationCapability` true (`DefaultPartyTemplate.ShipHulls.Count > 0`).

---

## How they get hired

No custom behavior is required.

### AI kingdoms

`DiplomaticBartersBehavior.DailyTickClan` (not the player clan, not eliminated, strength > 0).

If the clan is not already in the “make peace” or “defect” branch, there is a **40%** chance to look for a contract (**20%** when `MapFaction.Leader` is the player). It picks a kingdom, weighted **10** for the clan culture and **1** for every other culture.

It then returns without hiring when any of these are true:

- Kingdom leader is the player, or the kingdom is eliminated.
- Clan is already a non-mercenary member of some kingdom (`Kingdom != null && !IsUnderMercenaryService`). A clan already under contract may switch.
- Same faction, at war, or `DiplomacyModel.IsAtConstantWar`.
- Any war party is in a map event.
- `ShouldStayInKingdomUntil` is still in the future.

For `IsMinorFaction` it builds `MercenaryJoinKingdomBarterable` and applies it when clan score + kingdom score **> 0**.

- Kingdom score (`GetScoreOfKingdomToHireMercenary`): **+100** per war-party slot under 12 across the kingdom, **+30** per settlement under 40. Zero when the kingdom already has 12+ party slots and 40+ settlements.
- Clan score (`GetScoreOfMercenaryToJoinKingdom`): `(offered pay − pay they want) × (CurrentTotalStrength + WarPartyLimit × 50) × 0.5`. **Always 0** when the kingdom leader is the player, so this daily path never volunteers for the player anyway (and the player-kingdom filter already returned).

Apply writes `MercenaryAwardMultiplier` from `MinorFactionsModel.GetMercenaryAwardFactorToJoinKingdom` and sets the kingdom:

```cs
ChangeKingdomAction.ApplyByJoinFactionAsMercenary(
    clan,
    kingdom,
    default(CampaignTime),
    Campaign.Current.Models.MinorFactionsModel.GetMercenaryAwardFactorToJoinKingdom(clan, kingdom));
```

`default(CampaignTime)` is zero. `StartMercenaryServiceAction.ApplyByDefault` sets `Kingdom`, `IsUnderMercenaryService`, and the award. If the clan was already a mercenary, it ends the old contract first.

### Player kingdom

Dialog line `player_want_to_hire_mercenary` (“I would like you to serve {PLAYER_FACTION} as mercenary”).

`conversation_player_want_to_hire_mercenary_on_condition` requires all of:

- Player’s map faction is a kingdom and **the player is its leader**.
- Conversation hero is not a prisoner, `IsMinorFactionHero`, and not in an army.
- Not at war, and the hero’s clan is not already in the player’s faction.
- The player kingdom already has **fewer than 3** clans with `IsUnderMercenaryService`.

The leader (not a junior lord) then accepts when relation with the player is **above −10**. Relation **≤ −10** refuses. They also refuse when the player does not have **20 ×** the demanded award factor in gold, or when that gold check passes but `Clan.PlayerClan.DebtToKingdom` is **above 1000**.

If they are already mercenaries elsewhere, the demanded factor is multiplied by **3/2** and `ApplyByLeaveKingdomAsMercenary` runs before the join. Join again passes `default(CampaignTime)`.

---

## How long they stay

There is no day count on the contract. The dialog that ends the **player’s** own mercenary service says the same thing: paid per battle, not for a fixed period (`player_want_to_end_mercenary_service_response`).

AI companies stay until a daily leave check succeeds. Two checks, both only when `ShouldStayInKingdomUntil.IsPast` (already true for these joins) and the clan is not the player clan.

### They choose to leave

Only on days that did **not** enter the “look for a contract” branch. Then a further **40%** roll (`ConsiderClanLeaveAsMercenary`) runs, and only if no war party is in a map event.

So they even look at leaving on about **24%** of days while serving an AI kingdom (`0.6 × 0.4`), and about **32%** of days while serving the player (`0.8 × 0.4`).

They leave when `GetScoreOfMercenaryToLeaveKingdom` **> 500**:

```
50 × min(days since LastFactionChangeTime, 200) − 5000 − joinScore
```

`joinScore` is `GetScoreOfMercenaryToJoinKingdom` for the kingdom they are already in (current `MercenaryAwardMultiplier` minus the pay they want, times strength).

- Serving the **player**: `joinScore` is 0, so the score passes 500 at **110 days** and rises until day 200.
- Serving an **AI** kingdom: high pay (positive `joinScore`) pushes that past 110 days. If `joinScore` stays at **4500 or more**, the 200-day cap never clears 500. Low pay makes them leave sooner.

The same “look for a contract” branch can also move them to another AI kingdom in one day when that barter scores above 0.

### The ruler is in debt

`ClanVariablesCampaignBehavior.DailyTickClan`: if the clan is not the player clan, it is under mercenary service, `RulingClan.DebtToKingdom` **> 10000**, and the stay timer is past, there is a **25%** chance each day to call `ChangeKingdomAction.ApplyByLeaveKingdomAsMercenary` with no score check. This does **not** look at who the kingdom leader is. The “leader is not the player” check is only on the 10% award refresh below.

While the kingdom leader is **not** the player, each day also has a **10%** chance to rewrite `MercenaryAwardMultiplier` from `GetMercenaryAwardFactorToJoinKingdom`. That changes `joinScore` the next time they think about leaving. The player’s own mercenary contract renews that number every **30** days (`PlayerMercenaryServiceNextRenewalDay`). AI companies hired by the player do not use that 30-day renew; their join score stays 0, so only the day count matters.

---

## Pay

`MercenaryAwardMultiplier` is denars per influence point. Daily gold (`DefaultClanFinanceModel.AddMercenaryIncome`):

```
Ceiling(clan.Influence × (1 / RevenueSmoothenFraction)) × MercenaryAwardMultiplier
```

`RevenueSmoothenFraction()` is **5**, so one fifth of current influence is converted, then multiplied by the award. The same amount is subtracted from `Kingdom.MercenaryWallet`.

The award itself is `DefaultMinorFactionsModel.GetMercenaryAwardFactorToJoinKingdom` (kingdom power, budget, how many mercenaries it already has, fief count, mercenary tier and gold, relation). Vlandian culture feat `VlandianRenownMercenaryFeat` adds **15%**. When the player is kingdom leader and has `Trade.ManOfMeans`, the factor is multiplied by `1 + PrimaryBonus`. That bonus is **−0.2**, so the company is **20% cheaper** to hire, not more expensive.

---

## C# lookup (optional)

You do not create the clan. After XML load:

```cs
Clan clan = Clan.All.FirstOrDefault(c => c.StringId == "ghilman");
bool hired = clan != null && clan.IsUnderMercenaryService;
```

`Clan.NonBanditFactions` includes minor clans. `Clan.BanditFactions` does not.

Force a contract (same call the AI barter uses):

```cs
int award = Campaign.Current.Models.MinorFactionsModel.GetMercenaryAwardFactorToJoinKingdom(clan, kingdom);
ChangeKingdomAction.ApplyByJoinFactionAsMercenary(clan, kingdom, default(CampaignTime), award);
```

End it:

```cs
ChangeKingdomAction.ApplyByLeaveKingdomAsMercenary(clan);
```

Cheat shape in `SandBoxViewCheats.MakeClanMercenaryOfKingdom`: `campaign.MakeClanMercenaryOfKingdom [clan] | [kingdom] | [days]`. The optional day count is the one place vanilla sets a real `ShouldStayInKingdomUntil`. The XML/AI/dialog paths do not.

---

## SubModule.xml

Same nodes as any clan mod. Hideouts and bandit cultures are not required.

```xml
<XmlNode>
  <XmlName id="Factions" path="clans"/>
</XmlNode>
<XmlNode>
  <XmlName id="NPCCharacters" path="npccharacters"/>
</XmlNode>
<XmlNode>
  <XmlName id="partyTemplates" path="partyTemplates"/>
</XmlNode>
```

---

## Checklist

Required:

1. `<Faction id="X" is_minor_faction="true" culture="Culture...." default_party_template="PartyTemplate.Y" tier="4">`
2. `is_clan_type_mercenary="true"` if it should behave as a mercenary company (skip noble defection)
3. Four `<template id="NPCCharacter....">` children, each an `occupation="Lord"` `is_template="true"` character
4. `MBPartyTemplate` whose troops exist
5. `initial_home_settlement` points at a real settlement

Not required: kingdom, heroes.xml, hideouts, `<relationship>`, C# spawn or hire code.

---

## Pitfalls

- **`is_bandit="true"`** sends the clan through hideout spawn, not mercenary hire. See [Bandit Clans](/modding/bandit_clans/).
- **No character templates** → `FailedAssert`, no lords, no parties, nothing to hire.
- **`is_minor_faction` without `is_clan_type_mercenary`** still gets the mercenary join/leave rolls. The flag is not the hire switch.
- **Player can only offer a contract with fewer than 3 mercenary clans** already in the kingdom. The AI daily hire has no cap of 3; extra mercenaries only reduce the award factor.
- **AI never offers itself to the player’s kingdom** (`kingdom.Leader == Hero.MainHero` returns first).
- **No fixed term.** Dialog and AI both pass `default(CampaignTime)`.
- **Serving the player, they start clearing the leave check at 110 days**, and only on the days the 32% roll lands, and not during a map event.
- **Ruling clan debt over 10000** can drop a non-player mercenary clan at 25% per day, including one serving the player’s kingdom. The player clan itself is skipped.
- **At war with a kingdom** blocks the AI from joining that kingdom.
- **Junior lords** do not accept the player’s hire line. The condition requires `Clan.Leader ==` the conversation hero.

---

## Native reference

| Clan | Culture | Party template |
| --- | --- | --- |
| `ghilman` | `darshi` | `kingdom_hero_party_mercenary_aserai_template` |
| `legion_of_the_betrayed` | `empire` | `kingdom_hero_party_mercenary_empire_template` |
| `skolderbrotva` | `nord` | `kingdom_hero_party_mercenary_sturgia_template` |
| `company_of_the_boar` | `vlandia` | `kingdom_hero_party_mercenary_vlandia_template` |

Other `is_minor_faction` clans in `spclans.xml` (Beni Zilal, and the commented guardians / chosen nodes) use the same hero-spawn path. They are mafia, sect, or nomad, not `is_clan_type_mercenary`.
