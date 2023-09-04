# Equipment


[TaleWorlds.Core.Equipment Class Reference](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_core_1_1_equipment.html)

``` cs
Equipment equipment = Hero.MainHero.BattleEquipment;
Equipment equipment = Hero.MainHero.CivilianEquipment;
```

## Public Methods

``` cs
bool IsValid
bool IsCivilian
bool IsEmpty

bool HasWeapon()
bool HasWeaponOfClass(WeaponClass weaponClass)

EquipmentElement Horse
Equipment Clone(bool cloneWithoutWeapons = false)
void FillFrom(Equipment sourceEquipment, bool useSourceEquipmentType = true)

float GetTotalWeightOfArmor(bool forHuman)
float GetTotalWeightOfWeapons()
float GetHeadArmorSum()
float GetHumanBodyArmorSum()
float GetLegArmorSum()
float GetArmArmorSum()
float GetHorseArmorSum()

ArmorComponent.HairCoverTypes HairCoverType
ArmorComponent.BeardCoverTypes BeardCoverType
ArmorComponent.HorseHarnessCoverTypes ManeCoverType
bool EarsAreHidden
bool MouthIsHidden
ArmorComponent.BodyMeshTypes BodyMeshType
ArmorComponent.BodyDeformTypes BodyDeformType
Equipment.UnderwearTypes GetUnderwearType(bool isFemale)

void AddEquipmentToSlotWithoutAgent(EquipmentIndex equipmentIndex, EquipmentElement itemRosterElement)

static bool IsItemFitsToSlot(EquipmentIndex slotIndex, ItemObject item)
EquipmentIndex GetWeaponPickUpSlotIndex(EquipmentElement itemRosterElement, bool isStuckMissile)
bool IsEquipmentEqualTo(Equipment other)
static Equipment GetRandomEquipmentElements(BasicCharacterObject character, bool randomEquipmentModifier, bool isCivilianEquipment = false, int seed = -1)
static void SwapWeapons(Equipment equipment, EquipmentIndex index1, EquipmentIndex index2)


```


## EquipmentIndex

```
None
WeaponItemBeginSlot
WeaponItemBeginSlot,
Weapon0 = 0,
Weapon1,
Weapon2,
Weapon3,
ExtraWeaponSlot,
NumAllWeaponSlots,
NumPrimaryWeaponSlots = 4,
NonWeaponItemBeginSlot,
ArmorItemBeginSlot = 5,
Head = 5,
Body,
Leg,
Gloves,
Cape,
ArmorItemEndSlot,
NumAllArmorSlots = 5,
Horse = 10,
HorseHarness,
NumEquipmentSetSlots
```

## EquipmentType
```
Invalid
Battle
Civilian
```

## UnderwearTypes
```
NoUnderwear
FullUnderwear
OnlyTop
```

## Get equipment

``` cs
EquipmentElement rosterElement = equipment[EquipmentIndex.HorseHarness];
```

## Remove equipment

``` cs
equipment[EquipmentIndex.HorseHarness] = EquipmentElement.Invalid;
```

## Set horse as 'lame'

``` cs
equipment[EquipmentIndex.ArmorItemEndSlot] = EquipmentElement.Invalid;
ItemModifier @object = MBObjectManager.Instance.GetObject<ItemModifier>("lame_horse");
equipment[EquipmentIndex.ArmorItemEndSlot] = new EquipmentElement(equipment[EquipmentIndex.ArmorItemEndSlot].Item, @object, null, false);
```

!!! question "ArmorItemEndSlot - horse modifier?"



## MissionEquipment

[TaleWorlds.MountAndBlade.MissionEquipment Class Reference](https://apidoc.bannerlord.com/v/1.1.0/class_tale_worlds_1_1_mount_and_blade_1_1_mission_equipment.html)


Equipment of the agent in the mission.

MissionEquipment [Agent.Equipment](/modding/agents/#missionequipment)

``` cs
this._weaponSlots = new MissionWeapon[5];
```

### Public methods

``` cs
MissionWeapon this[int index]
MissionWeapon this[EquipmentIndex index]
FillFrom(Equipment sourceEquipment, Banner banner)
float GetTotalWeightOfWeapons()

bool HasShield()
bool HasAnyWeapon()
bool HasAnyWeaponWithFlags(WeaponFlags flags)
bool HasRangedWeapon(WeaponClass requiredAmmoClass = WeaponClass.Undefined)
bool ContainsNonConsumableRangedWeaponWithAmmo()
bool ContainsMeleeWeapon()
bool ContainsShield()
bool ContainsSpear()
bool ContainsThrownWeapon()

ItemObject GetBanner()

bool HasAmmo(EquipmentIndex equipmentIndex, out int rangedUsageIndex, out bool hasLoadedAmmo, out bool noAmmoInThisSlot)
int GetAmmoAmount(WeaponClass ammoClass)
int GetAmmoSlotIndexOfWeapon(WeaponClass ammoClass)
int GetMaxAmmo(WeaponClass ammoClass)
void GetAmmoCountAndIndexOfType(ItemObject.ItemTypeEnum itemType, out int ammoCount, out EquipmentIndex eIndex, EquipmentIndex equippedIndex = EquipmentIndex.None)
void CheckLoadedAmmos()

void SetUsageIndexOfSlot(EquipmentIndex slotIndex, int usageIndex)
void SetReloadPhaseOfSlot(EquipmentIndex slotIndex, short reloadPhase)
void SetAmountOfSlot(EquipmentIndex slotIndex, short dataValue, bool addOverflowToMaxAmount = false)
void SetHitPointsOfSlot(EquipmentIndex slotIndex, short dataValue, bool addOverflowToMaxHitPoints = false)
void SetReloadedAmmoOfSlot(EquipmentIndex slotIndex, EquipmentIndex ammoSlotIndex, short totalAmmo)
void SetConsumedAmmoOfSlot(EquipmentIndex slotIndex, short count)
void AttachWeaponToWeaponInSlot(EquipmentIndex slotIndex, ref MissionWeapon weapon, ref MatrixFrame attachLocalFrame)
void SetGlossMultipliersOfWeaponsRandomly(int seed)

static EquipmentIndex SelectWeaponPickUpSlot(Agent agentPickingUp, MissionWeapon weaponBeingPickedUp, bool isStuckMissile)
static bool DoesWeaponFitToSlot(EquipmentIndex slotIndex, MissionWeapon weapon)
```


### [Agent's](/modding/agents) actions

``` cs
Agent.DropItem(EquipmentIndex itemIndex, WeaponClass pickedUpItemType = WeaponClass.Undefined)
EquipItemsFromSpawnEquipment(bool neededBatchedItems)
EquipWeaponToExtraSlotAndWield(ref MissionWeapon weapon)
RemoveEquippedWeapon(EquipmentIndex slotIndex)

```
