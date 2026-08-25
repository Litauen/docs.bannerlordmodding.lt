# Custom Bandit Clans

!!! info "v1.4.8"

How to add a new bandit clan that actually spawns parties and uses hideouts.

Bandit clans are **not created in C#**. The game loads `<Faction>` XML, `Clan.Deserialize` builds the object, and vanilla `BanditSpawnCampaignBehavior` (land) / `PiratesCampaignBehavior` (War Sails sea) spawn parties later.

If a bandit faction exists but has **no hideout of the same culture** on the map, new game can crash in `BanditSpawnCampaignBehavior.GetInfestedHideoutCount`. See [Crashes](/modding/crashes/).

Related: [Clans](/modding/clans/), [Cultures](/modding/cultures/), [Hideouts](/modding/hideouts/), [settlements.xml](/modding/settlements_xml/), [Custom Culture](/guides/custom_culture/).

---

## The chain

A working hideout bandit needs all four layers. Missing one usually means the clan exists in encyclopedia but never appears on the map — or the game crashes on new campaign.

```
Faction  (is_bandit="true")
  → Culture  (is_bandit + can_have_settlement)
       → troop slots + boss party template
  → default_party_template
  → Hideouts in settlements.xml with the same culture
```

Hideouts are matched by **culture**, not by clan id and not by `initial_home_settlement`.

---

## 1. Faction XML

Load a `Factions` XML from your module (`SubModule.xml` → `XmlName id="Factions"`). Native file is `spclans.xml`.

```xml
<Factions>
  <Faction
    id="my_bandits"
    banner_key="17.181.116.1536.1536.768.768.1.0.0"
    color="FF8B7C73"
    color2="FF8B7C73"
    culture="Culture.my_bandits"
    default_party_template="PartyTemplate.my_bandits_template"
    settlement_banner_mesh="none"
    is_bandit="true"
    is_outlaw="true"
    name="{=MyBandits}My Bandits"
    initial_home_settlement="Settlement.hideout_my_1"
    tier="1">
  </Faction>
</Factions>
```

| Attribute | Effect |
|---|---|
| `id` | `Clan.StringId`. Used when creating parties (`my_bandits_1`, …). |
| `is_bandit="true"` | `Clan.IsBanditFaction` → appears in `Clan.BanditFactions`. |
| `is_outlaw="true"` | `Clan.IsOutlaw`. |
| `culture` | Must already exist. Hideout spawn keys off this. |
| `default_party_template` | Roaming / hideout party stack. |
| `initial_home_settlement` | `SetInitialHomeSettlement` only. Native points this at a **hideout**. A town/castle works as encyclopedia home but does **not** assign hideouts. |
| `banner_key`, `color`, `color2`, `name`, `tier` | Visuals / encyclopedia. |
| `owner` | Not needed. Bandit clans have no lords. |

Child `<relationship>` nodes are optional. Native bandit clans usually have none. War against everyone is implicit: `FactionManager` treats `IsBanditFaction` vs a non-outlaw faction as at war even with no stance XML.

Do **not** set `is_minor_faction` on a hideout bandit. Minor / mercenary clans are a different spawn path.

---

## 2. Culture XML

Native hideout cultures live in `spcultures.xml` (`forest_bandits`, `sea_raiders`, `mountain_bandits`, `desert_bandits`, `steppe_bandits`). Looters are a separate culture with `can_have_settlement="false"`.

```xml
<SPCultures>
  <Culture
    id="my_bandits"
    name="{=MyBandits}My Bandits"
    is_bandit="true"
    can_have_settlement="true"
    basic_troop="NPCCharacter.my_bandits_bandit"
    elite_basic_troop="NPCCharacter.my_bandits_raider"
    bandit_bandit="NPCCharacter.my_bandits_bandit"
    bandit_raider="NPCCharacter.my_bandits_raider"
    bandit_chief="NPCCharacter.my_bandits_chief"
    bandit_boss="NPCCharacter.my_bandits_boss"
    encounter_background_mesh="encounter_forest_bandit"
    bandit_boss_party_template="PartyTemplate.my_bandits_boss_party_template">
    <banner_bearer_replacement_weapons>
      <item id="Item.battania_sword_1_t2" />
      <item id="Item.battania_sword_2_t3" />
    </banner_bearer_replacement_weapons>
  </Culture>
</SPCultures>
```

### `can_have_settlement` — land vs looter vs pirate

This flag is the spawn switch. Combined with ships on the clan's party template:

| Culture `can_have_settlement` | Party template has `<ship_hulls>` | Treated as |
|---|---|---|
| `true` | no | Hideout bandit (`BanditSpawnCampaignBehavior`) |
| `false` | no | Looter — roam near towns/villages, hideouts stay empty |
| `false` | yes | War Sails pirate (`PiratesCampaignBehavior`) |
| `true` | yes | **Neither.** Land skip (has ships). Sea skip (`CanHaveSettlement`). Clan exists, almost never spawns. |

Native `looters` / `deserters`: `can_have_settlement="false"`, no ships.

`Clan.HasNavalNavigationCapability` is simply `DefaultPartyTemplate.ShipHulls.Count > 0`.

One clan per hideout culture. `AddBanditToHideout` loops every `Clan.BanditFactions` with that culture and keeps the **last** match. A second clan on the same culture (for example a “pirate” clone) can steal hideout assignment.

---

## 3. Party templates

```xml
<partyTemplates>
  <MBPartyTemplate id="my_bandits_template">
    <stacks>
      <PartyTemplateStack min_value="4" max_value="36" troop="NPCCharacter.my_bandits_bandit" />
      <PartyTemplateStack min_value="1" max_value="12" troop="NPCCharacter.my_bandits_raider" />
      <PartyTemplateStack min_value="0" max_value="6" troop="NPCCharacter.my_bandits_chief" />
    </stacks>
  </MBPartyTemplate>

  <MBPartyTemplate id="my_bandits_boss_party_template">
    <stacks>
      <PartyTemplateStack min_value="14" max_value="30" troop="NPCCharacter.my_bandits_bandit" />
      <PartyTemplateStack min_value="10" max_value="20" troop="NPCCharacter.my_bandits_raider" />
      <PartyTemplateStack min_value="1" max_value="10" troop="NPCCharacter.my_bandits_chief" />
    </stacks>
  </MBPartyTemplate>
</partyTemplates>
```

- Clan `default_party_template` = roaming parties and hideout fill.
- Culture `bandit_boss_party_template` = boss party when a spotted hideout has bandits but no boss.
- Troop `culture=""` on the `NPCCharacter` does **not** have to match the bandit culture. Hideout pick uses the **hideout settlement's** culture, then fills from the clan/culture templates.
- For land bandits, do **not** add `<ship_hulls>`.

Pirate template (War Sails) — culture must be `can_have_settlement="false"`:

```xml
<MBPartyTemplate id="my_pirates_template">
  <stacks>
    <PartyTemplateStack min_value="20" max_value="35" troop="NPCCharacter.sea_raiders_bandit" />
    <PartyTemplateStack min_value="15" max_value="25" troop="NPCCharacter.sea_raiders_raider" />
  </stacks>
  <ship_hulls>
    <ShipTemplateStack min_value="2" max_value="3" id="ShipHull.longship" />
  </ship_hulls>
</MBPartyTemplate>
```

Native War Sails pirate clans (`northern_pirates`, `southern_pirates`) use their own cultures with `can_have_settlement="false"`. Do not reuse a hideout bandit culture on a pirate clan.

---

## 4. Hideouts

Hideouts are settlements with a `<Hideout>` component. Native examples: `hideout_forest_1`, `hideout_seaside_6`, …

```xml
<Settlement id="hideout_my_1" name="{=hideout}Hideout" type="Hideout"
            posX="339.098" posY="576.964" culture="Culture.my_bandits">
  <Components>
    <Hideout id="hideout_my_1"
             map_icon="bandit_hideout_b"
             background_crop_position="0.0"
             background_mesh="empire_twn_scene_bg"
             wait_mesh="wait_hideout_forest"
             gate_rotation="0.0" />
  </Components>
  <Locations complex_template="LocationComplexTemplate.hideout_complex">
    <Location id="hideout_center" scene_name="bandit_forest" />
  </Locations>
</Settlement>
```

`culture` must be the **same id** as the bandit clan's culture. That is the only link.

Hideout `MapFaction` is not set in XML. At runtime it is the first bandit party inside, or else the first `IsBanditFaction` clan in `Clan.All`.

`type="Hideout"`, `map_icon`, and `gate_rotation` are present on native hideouts; they are not what the spawn behavior uses. See [settlements.xml](/modding/settlements_xml/).

Give the faction enough hideouts. Native `DefaultBanditDensityModel` (1.2–1.4):

| Value | Default |
|---|---|
| Parties needed to infest a hideout | 2 |
| Max parties inside a hideout | 3 |
| Max parties around each hideout | 3 |
| Initial hideouts per faction (new game) | 7 |
| Max hideouts per faction | 9 |

If you only place 3 hideouts, the clan can never reach the native initial count. War Sails / StoryMode can wrap this model and change the numbers.

Duplicate hideout **settlement ids** break `initial_home_settlement` lookups (native `looters` points at `hideout_mountain_2`).

---

## How parties appear

No custom behavior is required.

New game (`BanditSpawnCampaignBehavior`):

1. Partial follow-up **10** — cache hideouts by culture. For each clan that is a hideout bandit (`IsBanditFaction` + `CanHaveSettlement` + no ships), infest `NumberOfInitialHideoutsAtEachBanditFaction` hideouts of that culture.
2. Follow-up **11** — spawn roaming parties around those hideouts from `clan.DefaultPartyTemplate`. Looter clans spawn near towns/villages instead.
3. Nightly `HourlyTickClan` — more parties around infested hideouts.

A hideout is infested when it has at least `NumberOfMinimumBanditPartiesInAHideoutToInfestIt` bandit parties.

`AddBanditToHideout`:

```
hideout.Settlement.Culture == clan.Culture
```

then `BanditPartyComponent.CreateBanditParty(...)`.

War Sails sea: `PiratesCampaignBehavior` only ticks clans where `!Culture.CanHaveSettlement && HasNavalNavigationCapability && IsBanditFaction`. Those parties are created as looter-style sea parties and patrol naval zones.

---

## C# lookup (optional)

You do not create the clan. After XML load you can find it:

```
Clan clan = Clan.BanditFactions.FirstOrDefault(c => c.StringId == "my_bandits");
PartyTemplateObject pt = MBObjectManager.Instance.GetObject<PartyTemplateObject>("my_bandits_template");
```

Creating extra parties:

```
MobileParty party = BanditPartyComponent.CreateBanditParty(
    clan.StringId + "_extra",
    clan,
    hideout,
    isBossParty: false,
    clan.DefaultPartyTemplate,
    hideout.Settlement.GatePosition);
```

---

## Optional GameText

Display-only. Not required for spawn. In `module_strings.xml` (or your GameText file), keys are `str_….cultureId`:

```
str_culture_description.my_bandits
str_adjective_for_culture.my_bandits
str_neutral_term_for_culture.my_bandits
str_culture_rich_name.my_bandits
str_faction_ruler.my_bandits
str_liege_title.my_bandits
```

`encounter_background_mesh` is the encounter menu picture (`encounter_forest_bandit`, `encounter_looter`, …).

---

## SubModule.xml

Typical nodes (same pattern as other XML):

```xml
<XmlNode>
  <XmlName id="Factions" path="clans"/>
  ...
</XmlNode>
<XmlNode>
  <XmlName id="SPCultures" path="cultures"/>
  ...
</XmlNode>
<XmlNode>
  <XmlName id="NPCCharacters" path="npc_troops"/>
  ...
</XmlNode>
<XmlNode>
  <XmlName id="partyTemplates" path="party_templates"/>
  ...
</XmlNode>
<XmlNode>
  <XmlName id="Settlements" path="settlements"/>
  ...
</XmlNode>
```

Folder paths load every XML in that folder. If you also XSLT-delete native clans (`clans.xslt`), do **not** delete `looters` / `forest_bandits` / … unless you replace their hideouts and anything that hardcodes those ids (issues, alleys, voiced lines).

---

## Checklist

Required:

1. `<Faction id="X" is_bandit="true" is_outlaw="true" culture="Culture.X" default_party_template="PartyTemplate.Y">`
2. `<Culture id="X" is_bandit="true" can_have_settlement="true">` with `bandit_*` troop slots and `bandit_boss_party_template` (land hideout) **or** `can_have_settlement="false"` for looters / pirates
3. `MBPartyTemplate` — no ships for land, ships for pirates
4. Hideouts with `culture="Culture.X"` (about 7+ if you want native density)
5. Every referenced `NPCCharacter` / `Settlement` / `PartyTemplate` id exists
6. One bandit clan per hideout culture

Not required: heroes, kingdom, `<relationship>` children, C# spawn code.

---

## Pitfalls

- **No hideouts of that culture** → crash in `GetInfestedHideoutCount` ([Crashes](/modding/crashes/)).
- **Two clans, one culture** → hideout fill keeps the last `Clan.BanditFactions` match.
- **Ships + `can_have_settlement="true"`** → skipped by both land and sea spawners.
- **Reusing a hideout culture on a pirate clan** → same gap as above; also steals hideout assignment.
- **Looter culture (`can_have_settlement="false"`, no ships)** → parties everywhere, hideouts empty. Native `looters`.
- **Too few hideouts** vs native 7 / 9 caps.
- **`initial_home_settlement` on a town** does not attach hideouts. Match hideouts by culture.
- **Troop culture mismatch** is legal. The hideout settlement culture is what matters for clan pick.
- **Sharing a party template with a minor mercenary clan** is legal. Different `is_bandit` / `is_minor_faction` flags, same stack.
- **Deleting native bandit troops with XSLT** without redefining the same ids breaks native clans that still load from `spclans.xml`.
- **Hardcoded native ids** in issues / alleys / voice-over (`forest_bandits`, `looters`, …) stay vanilla even if you add new clans.

---

## Native reference

| Clan / culture | Role |
|---|---|
| `looters` | Roaming looters. `can_have_settlement="false"`. |
| `deserters` | Same culture family as looters; excluded from the looter spawn helper. |
| `forest_bandits`, `sea_raiders`, `mountain_bandits`, `desert_bandits`, `steppe_bandits` | Hideout bandits. |
| `northern_pirates`, `southern_pirates` | War Sails sea. Own cultures, `can_have_settlement="false"`, templates have ships. |
