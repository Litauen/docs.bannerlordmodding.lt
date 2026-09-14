# Ships

War Sails (`NavalDLC`). Campaign object is `TaleWorlds.CampaignSystem.Naval.Ship`. Its type / stats come from `ShipHull` XML.

* [Ship Creation](https://moddocs.bannerlord.com/war-sails/ship_creation/) — mission prefab / physics / boarding pipeline (editor). This page is campaign XML + C#.

``` cs
ShipHull hull = MBObjectManager.Instance.GetObject<ShipHull>("northern_light_ship");
Ship ship = new Ship(hull);   // full HP + sail HP, empty upgrade slots
```

## Objects

``` cs
Ship            // saved instance owned by a PartyBase (mobile party or settlement)
ShipHull        // MBObject from ModuleData/ship_hulls.xml
ShipSlot        // ModuleData/ship_slots.xml
ShipUpgradePiece
Figurehead      // DefaultFigureheads (C#, not XML)
AnchorPoint     // MobileParty.Anchor — fleet left on the map when you disembark
```

A `Ship` is not an inventory item. Owner is `PartyBase`:

``` cs
MobileParty.MainParty.Ships          // MBReadOnlyList<Ship>
PartyBase.MainParty.Ships
Settlement.CurrentSettlement.Party.Ships
town.AvailableShips                  // same list: Town.AvailableShips => Settlement.Party.Ships
PartyBase.MainParty.FlagShip          // Ships.MaxBy(FlagshipScore)
```

Do not add to `_ships` yourself. Set ownership through `ChangeShipOwnerAction` (or `ship.Owner = …`, which only updates the list and skips gold / events).

## XML load order

`NavalDLCSubModule.RegisterSubModuleObjects`:

```
ShipUpgradePieces
ShipSlots
ShipHulls
ShipPhysicsReferences
MissionShips
```

Hull `mission_ship="…"` is the `MissionShips` / nested-prefab id (not the hull StringId).

Files (vanilla module):

```
Modules\NavalDLC\ModuleData\ship_hulls.xml
Modules\NavalDLC\ModuleData\ship_slots.xml
Modules\NavalDLC\ModuleData\ship_upgrade_pieces.xml
Modules\NavalDLC\ModuleData\ship_physics_references.xml
Modules\NavalDLC\ModuleData\mission_ships.xml
```

## ShipHull

``` xml
<ShipHull
    id="northern_light_ship"
    name="{=bvfd7xzX}Light Longship"
    description="{=…}"
    mission_ship="ship_lightlongship"
    skeletal_crew_capacity="8"
    main_deck_crew_capacity="32"
    total_crew_capacity="32"
    max_hitpoints="9000"
    max_fire_hitpoints="9000"
    max_sail_hitpoints="4500"
    ship_type="light"
    has_hold="false"
    base_speed="3"
    default_group="Infantry"
    can_navigate_shallow_water="true"
    can_equip_figurehead="true"
    production_build_weight="4.0"
    value="8500"
    sea_worthiness="35"
    inventory_capacity="1800"
    map_visual_scale="0.06">
    <AvailableSlots>
        <ShipSlot id="side" tag_id="side" />
        <ShipSlot id="deck" tag_id="deck" />
        <ShipSlot id="oars" tag_id="oars" />
        <ShipSlot id="sail" tag_id="sail" />
    </AvailableSlots>
</ShipHull>
```

`AvailableSlots/@id` is a `ShipSlot` StringId. `tag_id` is the prefab child tag used at runtime (`GetPieceAtSlot(tag_id)`).

`ship_type`: `Light` / `Medium` / `Heavy` (`ShipHull.ShipType`). Town production uses this vs shipyard level.

Trade hulls also set `is_trade_ship="true"` (Knarr, Dhow, Trade Cog, Corbita).

### Vanilla campaign hulls

Playable / stock hulls from `ship_hulls.xml` (English name in parentheses). Storyline-only ids (`*_storyline`, `burning_*`, `fishing_ship`, …) exist in the same file but are not in Custom Battle / culture stock lists.

| StringId | Name | Type |
|---|---|---|
| `northern_light_ship` | Light Longship | light |
| `northern_medium_ship` | Longship | medium |
| `nord_medium_ship` | Drakkar | heavy |
| `nord_mediumballista_ship` | Battle Knarr | medium |
| `sturgia_heavy_ship` | Lodya | heavy |
| `northern_trade_ship` | Knarr | light, trade |
| `western_light_ship` | Western Galley | light |
| `western_medium_ship` | Cog | medium |
| `vlandia_heavy_ship` | Roundship | heavy |
| `western_trade_ship` | Trade Cog | medium, trade |
| `battanian_light_ship` | Birlinn | light |
| `battanian_medium_ship` | Barlinnger | medium |
| `central_light_ship` | Eastern Galley | light |
| `empire_medium_ship` | Liburna | medium |
| `empire_heavy_ship` | Dromon | heavy |
| `empire_trade_ship` | Corbita | medium, trade |
| `eastern_medium_ship` | Sambuk | medium |
| `eastern_heavy_ship` | Dromakion | heavy |
| `aserai_heavy_ship` | Ghurab | heavy |
| `khuzait_heavy_ship` | Qalguk | heavy |
| `eastern_trade_ship` | Dhow | light, trade |
| `southern_fishing_ship` | Southern Fishing Ship | light |
| `fishing_ship` | Fishing Ship | light |

Custom Battle picker (`NavalCustomBattleData.ShipHulls`) is a hard-coded subset of the table above (no fishing hulls).

## Culture stock

`SandBoxCore` cultures ship an empty node:

``` xml
<available_ship_hulls></available_ship_hulls>
```

War Sails XSLT (`NavalDLC_SandBoxCore_SPCultures.xslt`) injects `<ship_hull id="ShipHull.…" />` children. Vanilla mapping:

| Culture | Hulls |
|---|---|
| empire | empire_heavy_ship, empire_medium_ship, central_light_ship, eastern_trade_ship, western_light_ship, eastern_heavy_ship, empire_trade_ship |
| aserai | central_light_ship, eastern_heavy_ship, aserai_heavy_ship, eastern_medium_ship, eastern_trade_ship |
| sturgia | northern_medium_ship, northern_light_ship, sturgia_heavy_ship, northern_trade_ship, nord_mediumballista_ship |
| vlandia | western_light_ship, western_medium_ship, vlandia_heavy_ship, western_trade_ship, battanian_medium_ship |
| battania | western_light_ship, western_medium_ship, battanian_light_ship, northern_trade_ship, western_trade_ship, battanian_medium_ship |
| khuzait | khuzait_heavy_ship, eastern_medium_ship, eastern_heavy_ship, central_light_ship, eastern_trade_ship |
| nord | northern_light_ship, northern_medium_ship, nord_medium_ship, northern_trade_ship, nord_mediumballista_ship |

A custom culture can fill the node itself (no XSLT):

``` xml
<available_ship_hulls>
    <ship_hull id="ShipHull.northern_light_ship" />
    <ship_hull id="ShipHull.northern_medium_ship" />
</available_ship_hulls>
```

``` cs
culture.AvailableShipHulls
```

Town production only creates hulls from that list, weighted by `production_build_weight`, gated by shipyard level:

* Light — shipyard `CurrentLevel > 0`
* Medium — `> 1`
* Heavy — `== 3`

## Party templates

`PartyTemplateObject.ShipHulls` is a list of `ShipTemplateStack` (`min_value` / `max_value` / hull). Same min/max idea as troop stacks — `DefaultPartySizeLimitModel.FindAppropriateInitialShipsForMobileParty` rolls a count in that range (party size ratio).

``` xml
<MBPartyTemplate id="northern_pirates_template">
    <stacks>
        <PartyTemplateStack min_value="20" max_value="35" troop="NPCCharacter.sea_raiders_bandit" />
        <PartyTemplateStack min_value="15" max_value="25" troop="NPCCharacter.sea_raiders_raider" />
        <PartyTemplateStack min_value="5" max_value="10" troop="NPCCharacter.sea_raiders_chief" />
    </stacks>
    <ship_hulls>
        <ShipTemplateStack min_value="2" max_value="3" id="ShipHull.northern_light_ship" />
        <ShipTemplateStack min_value="1" max_value="1" id="ShipHull.northern_medium_ship" />
    </ship_hulls>
</MBPartyTemplate>
```

Vanilla file: `Modules\NavalDLC\ModuleData\naval_partyTemplates.xml` (pirates, naval caravans, patrols, fishing, storyline).

When a party is created from a template:

``` cs
foreach (Ship item in Campaign.Current.Models.PartySizeLimitModel
    .FindAppropriateInitialShipsForMobileParty(party, pt))
{
    ChangeShipOwnerAction.ApplyByMobilePartyCreation(party.Party, item);
}
```

No `<ship_hulls>` → party starts with zero ships.

``` cs
clan.HasNavalNavigationCapability      // DefaultPartyTemplate.ShipHulls.Count > 0
kingdom.HasNavalNavigationCapability    // Culture.DefaultPartyTemplate.ShipHulls.Any()
```

Used by pirate spawn (`PiratesCampaignBehavior`): bandit clan + `!Culture.CanHaveSettlement` + template ships. See [Bandit Clans](/modding/bandit_clans/).

Caravan templates: vanilla picks a land or naval template from `Culture.CaravanPartyTemplates` depending on `settlement.HasPort`.

## Create / give a ship

``` cs
ShipHull hull = MBObjectManager.Instance.GetObject<ShipHull>("northern_light_ship");
if (hull == null) return;

Ship ship = new Ship(hull);
ship.SetName(new TextObject("Sea Wolf"));
ChangeShipOwnerAction.ApplyByLooting(PartyBase.MainParty, ship);

if (!MobileParty.MainParty.IsCurrentlyAtSea && !MobileParty.MainParty.Anchor.IsValid)
{
    IEnumerable<Town> ports = Town.AllTowns.Where(t => t.Settlement.HasPort);
    Settlement port = TaleWorlds.Core.Extensions.MinBy(ports,
        (Town t) => t.Settlement.PortPosition.Distance(MobileParty.MainParty.Position)).Settlement;
    MobileParty.MainParty.Anchor.SetSettlement(port);
}
```

Vanilla cheat `naval.add_ship_to_player` does the same (`ApplyByLooting` + nearest port anchor).

`new Ship(hull)` sets `HitPoints = MaxHitPoints` and `SailHitPoints = MaxSailHitPoints`. Slots start empty (`null` piece).

Assigning `ship.Owner` directly also moves the ship between lists, but **does not** fire `OnShipOwnerChanged` or pay gold. Prefer the action.

## Change owner

``` cs
ChangeShipOwnerAction.ApplyByTransferring(newOwner, ship);         // no gold
ChangeShipOwnerAction.ApplyByTrade(newOwner, ship);                // gold via ShipCostModel
ChangeShipOwnerAction.ApplyByLooting(newOwner, ship);
ChangeShipOwnerAction.ApplyByProduction(newOwner, ship);           // town shipyard
ChangeShipOwnerAction.ApplyByMobilePartyCreation(newOwner, ship);
```

`newOwner` is `PartyBase` (party or settlement). Trade from a settlement also sets `Anchor` on the buyer if they had no ships / invalid anchor.

Flags on the instance:

``` cs
ship.IsTradeable     // default true — AI / port will sell it
ship.IsUsedByQuest
ship.IsInvulnerable  // OnShipDamaged returns 0
```

## Party at sea

``` cs
MobileParty.MainParty.HasNavalNavigationCapability
    // NavalPartyNavigationModel: Ships.Count > 0
    // (or attached to / leading a fleet that has ships; main party: ships only)

MobileParty.MainParty.IsCurrentlyAtSea
MobileParty.MainParty.IsInRaftState
MobileParty.MainParty.Anchor              // AnchorPoint
MobileParty.MainParty.Anchor.IsValid
MobileParty.MainParty.Anchor.SetSettlement(settlement);   // PortPosition + GatePosition
MobileParty.MainParty.Anchor.SetPosition(pos);
MobileParty.MainParty.Anchor.CallFleet(settlement);
MobileParty.MainParty.SetNavalVisualAsDirty();
```

`Clan.HasNavalNavigationCapability` is template-based. `MobileParty.HasNavalNavigationCapability` is the live ship list.

Without ships at sea → raft state (`RaftStateCampaignBehavior`), not a working fleet.

## Hit points

``` cs
ship.HitPoints
ship.MaxHitPoints          // hull + upgrade MaxHitPointsBonusMultiplier
ship.SailHitPoints
ship.MaxSailHitPoints
ship.MaxFireHitPoints
```

``` cs
ship.OnShipDamaged(rawDamage, rammingShip, out float modifiedDamage);
```

`CampaignShipDamageModel.GetShipDamage` then subtracts. At `HitPoints <= 0` it calls `DestroyShipAction.Apply(this)`.

``` cs
ship.GetCampaignSpeed()    // ShipHull.BaseSpeed * (1 + CampaignSpeedBonusFactor)
ship.GetCombatFactor()
ship.TotalCrewCapacity
ship.InventoryCapacity
ship.SeaWorthiness
```

## Repair

``` cs
RepairShipAction.Apply(ship, repairPort);     // AI caravan / lord: pays ShipCostModel.GetShipRepairCost
RepairShipAction.ApplyForFree(ship);
RepairShipAction.ApplyForBanditShip(ship);    // cap at 80% if below 80%
```

Moving a ship onto a settlement (`ApplyByTrade` / production owner-change) also free-repairs it (`ShipProductionCampaignBehavior.OnShipOwnerChanged`).

## Destroy

``` cs
DestroyShipAction.Apply(MobileParty.MainParty.Ships[i]);
DestroyShipAction.ApplyByDiscard(ship);
```

Clears `Owner`, dirties naval visuals, fires `OnShipDestroyed`. Do not only `ship.Owner = null` if you need the event (discard salvage perks, town stock cleanup, …).

Iterate the list **backwards** if you destroy while looping (`OnShipDamaged` can destroy mid-loop).

## Upgrades

Slots live on the hull. Pieces are `ShipUpgradePiece` XML.

``` cs
ship.HasSlot("sail")
ship.GetPieceAtSlot("sail")
ship.EquipUpgradePiece("sail", piece);   // MainParty: also unlocks the piece for that ship
ship.GetSiegeEngines()                     // pieces with SiegeEngine (ballistae, ram, …)
```

Vanilla slot StringIds include `fore`, `medium_fore`, `aft`, `bow`, `side`, `deck`, `oars`, `sail`, `hull`, `roof`, …

``` xml
<ShipUpgradePiece
    id="bow_ram_lvl1"
    name="{=shipUpgradeRam}Ram"
    description="{=…}"
    slot_prefab_child_id="ram_01"
    light_value="350"
    medium_value="700"
    heavy_value="1400"
    siege_engine="SiegeEngineType.bow_ram_lvl1"
    required_port_level="1"
    required_culture_1_id="Culture.empire"
    required_culture_2_id="Culture.aserai"
    ship_weight_bonus_multiplier="0.05">
    <TargetSlots>
        <ShipSlot id="bow" />
    </TargetSlots>
</ShipUpgradePiece>
```

Cost uses `light_value` / `medium_value` / `heavy_value` from the **hull** `ship_type`. `required_port_level` is shipyard level. `not_merchandise="true"` hides a piece from town stock.

Vanilla attribute name typo (keep if you copy): `furl_unfurl_speed_bonus_multipler` (one `p`).

Town pieces: `town.GetAvailableShipUpgradePieces()` — not merchandise, `RequiredPortLevel <= shipyard`, culture match (`RequiredCulture1` / `2` or both null).

## Figureheads

Not XML. `DefaultFigureheads` registers `Figurehead` objects (`hawk`, `lion`, `dragon`, …).

``` cs
ship.CanEquipFigurehead          // ShipHull.CanEquipFigurehead
ship.ChangeFigurehead(DefaultFigureheads.Dragon);
ship.Figurehead
```

Vanilla: Hawk (aserai), Lion (vlandia), Dragon (nord), Wings Of Victory / Ram / Swan (empire), Sea Serpent / Raven (nord), Viper (aserai), Saber Tooth Tiger (vlandia), Siren / Turtle / Deer (sturgia), Horse (khuzait), Boar / Oxen (battania).

Prefab slot tag is `"figurehead"` (`NavalDLCSubModule.FigureheadSlotTag`).

Cheat: `naval.unlock_figurehead [id|all]`.

## Port / shipyard

Port on the settlement (world-map `town_port` / `PortPosition`), not the building:

``` cs
settlement.HasPort              // PortPosition.ToVec2() != Vec2.Zero
settlement.PortPosition
town.GetShipyard()              // BuildingType building_shipyard
```

Shipyard (`NavalBuildingTypes.SettlementShipyard`, id `building_shipyard`):

| Level | Production | Max ships in town |
|---|---|---|
| 1 | Light | 9 |
| 2 | Medium | 12 |
| 3 | Heavy | 15 |

`BuildingEffectEnum.MaximumShipCount` / `ShipProduction`. Daily: `ShipProductionCampaignBehavior` may create up to 10 ships if under cap (`0.5` chance, Boatswain Streamlined Operations perk). Excess / wrong-culture hulls are discarded.

Open the port UI:

``` cs
PortStateHelper.OpenAsTrade(town);
PortStateHelper.OpenAsLoot(lootShips);
PortStateHelper.OpenAsManageFleet(leftShips);
PortStateHelper.OpenAsManageOtherFleet(otherParty, onEnd);
PortStateHelper.OpenAsRestricted(town, reason);
PortStateHelper.OpenAsStoryMode(settlement);
```

`PortScreenModes`: `Story`, `Restricted`, `TradeMode`, `LootMode`, `Manage`, `ManageOther`.

## Events

``` cs
CampaignEvents.OnShipCreatedEvent          // Ship, Settlement
CampaignEvents.OnShipDestroyedEvent         // PartyBase owner, Ship, ShipDestroyDetail
CampaignEvents.OnShipOwnerChangedEvent      // Ship, PartyBase oldOwner, ShipOwnerChangeDetail
CampaignEvents.OnShipRepairedEvent         // Ship, Settlement repairPort
```

## Models

NavalDLC replaces the campaign stubs (`DefaultShipCostModel` etc. return 0).

``` cs
Campaign.Current.Models.ShipCostModel
    .GetShipTradeValue(ship, seller, buyer)
    .GetShipRepairCost(ship, owner)
    .GetShipUpgradePieceCost(ship, piece, owner)
    .GetShipSellingPenalty()

Campaign.Current.Models.ShipStatModel.GetShipFlagshipScore(ship)
Campaign.Current.Models.PartyShipLimitModel.GetIdealShipNumber(party)   // lords/caravans/bandits: 3
Campaign.Current.Models.CampaignShipParametersModel
Campaign.Current.Models.CampaignShipDamageModel
```

Buy value is roughly hull `value` × `1.5` plus equipped pieces (`NavalDLCShipCostModel`). Sell to a town uses a `0.3` sell penalty minus repair cost. AI clans buying from a town get a heavy discount (`0.01`).

## Cheats

```
naval.add_ship_to_player [ShipName] | [Count]
naval.damage_player_ships [fraction]
naval.unlock_figurehead [figurehead_id|all]
```

`ShipName` is matched against hulls on `Kingdom.All` → `Culture.AvailableShipHulls` (not storyline-only ids).

## Custom Battle

Land Custom Battle has no ships. Naval Custom Battle reads `NavalCustomBattleData.ShipHulls`. Extra hulls: Harmony postfix that `Concat`s your `ShipHull` — see [Custom Battle Faction](/guides/custom_battle_faction/).
