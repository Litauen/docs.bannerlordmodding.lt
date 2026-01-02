# settlements.xml

## 1.3 changes related to DLC

`<Settlement` (Top Level) attributes:

* port_posX
* port_posY

`<Buildings>`:

* `<Building id="building_shipyard" level="1"/>`

`<Locations>`:

* `<Location id="port" scene_name="empire_shipyard"/>`

## Deserialize

- **If there is no note about omission of an attribute, assume that it must be present - most of these have no null protection during read.**
- Attributes are in the order listed in the deserialize method which differs from the xml ordering.
- If an attribute is read separate from the others in its node, I've grouped it with them anyways, eg. ***owner*** is after all of the child nodes, but it's less clear if I imitate that ordering.
- Attribute names are bold and italicized here.

### `<Settlement` (Top Level) attributes
- ***id*** : used as the StringId in MBObjectManager
- ***name*** : string from xml is placed into a new TextObject - must include localization id in the string
- ***type*** : present in native xml, but seems unused
	- this is present on hideouts, eg. `type="Hideout"`, but I see nowhere that this attribute is read, stored, or fetched
- ***posX*** : value is converted to a double, then cast to a float
- ***posY*** : value is converted to a double, then cast to a float
	- ***posX*** and ***posY*** are stored in the settlement.\_position field as a CampaignVec2 with isOnLand = true
	- the LocatorGrid for settlements is updated when the property setter is used
- ***gate_posX*** : value is converted to a double, then cast to a float
- ***gate_posY*** : value is converted to a double, then cast to a float
	- GatePosition by default is the same as the settlement's Position (same coordinates, isOnLand = true) so this can be omitted safely
	- the coordinates are only updated if ***gate_posX*** and ***gate_posY*** exist in the xml
- ***port_posX*** : value is converted to a double, then cast to a float
- ***port_posY*** : value is converted to a double, then cast to a float
	- PortPosition by default is the same as the settlement's Position (same coordinates, but with isOnLand = false) so this can be omitted safely
		- however, this means every single settlement has a valid port position including villages and hideouts
	- the coordinates are only updated if ***port_posX*** and ***port_posY*** exist in the xml
		- settlement.HasPort = true when the PortPosition != Vec2.Zero **and** when both port positions have a valid number in the xml
			- consequently, checking a settlement for a non-zero port position is *insufficient* because it will consider every settlement to be a port settlement. You must check HasPort beforehand to conclude if a settlement actually has a port.
- ***culture*** : the id is looked up in MBObjectManager and the relevant object is stored on the settlement
	- the format *must be* : `"Culture.(culture's StringId)"` as the string is split at the period :
		- `Culture` is used to find the object type in the manager
		- the `StringId` is used to find the corresponding object within that type
- ***text*** : the settlement's EncyclopediaText is set to the value if this attribute exists
	- if omitted, an empty TextObject is assigned instead


### `<Components>` (`Settlement` first child node)
- Valid Component Names : 
	- native options are `Town`, `Village`, or `Hideout`
	- use `CustomSettlementComponent` for modded components
		- a `component_name` attribute is used for the Type of the component, eg.
			~~~
			<CustomSettlementComponent
				component_name = "Shrine"
				id = "sigmar_shrine_01"
			~~~
			- this will create the PresumedObject for a Shrine-typed settlement with a StringId of `sigmar_shrine_01`
				- use MBObjectBase.AfterInitialized if you need to work with the object after its creation
		- deserialization of the contents of the custom component must be performed by a method you provide
#### `<Town` (SettlementComponent)
- ***id*** : the StringId in MBObjectManager
	- generally follows the id of the parent node, eg. `town_comp_EN1` is the town component id for the settlement `town_EN1`
- ***is_castle*** : used to check if a Town component belongs to a castle or a town (city)
	- (why yes, that is confusing nomenclature)
	- if omitted, defaults to `false`
- ***background_crop_position*** : parsed as a float
	- (but cast to double when used? why are other things parsed as double and casted to floats then?)
	- used in EncyclopediaPages and Kingdom SettlementDecisions to change the cropping for the settlement image in the VM
- ***background_mesh*** : string that is usually for creating a file name used in widgets
	- eg. EncyclopediaSettlementVM.FileName is set to `(BackgroundMeshName)_t`, and the widget's SpriteBrushLayerName is the resulting name for looking up the sprite
- ***wait_mesh*** : string to find the image via MenuContextHandler
	- default image for the encounter menu of the settlement
		- there's so many usages as BackgroundMesh that are unrelated to "waiting" that this is just "background_mesh" for the encounter menu in the settlement
- ***prosperity*** : starting prosperity for a settlement on campaign creation as a float
	- Campaign.GameLoadingType.SavedCampaign is checked to know if the campaign has already been created
- ***level*** : THIS NO LONGER EXISTS
	- see the \_fortifications `Building`'s ***level*** 
##### `Buildings` (child node of `Town`)
- In turn, this has a set of child nodes called `Building`(singular) which define the starting levels for building types supported by a given component
	- the deserialize does *NOT* call BuildingModel's CanAddBuildingTypeToTown - consequently, castle specific buildings and town specific buildings will not throw any errors if they're added to a type they should not be present on
		- perks that affect building bonuses are based on the building type - if you put a CastleGranary on a town, Engineering.Battlements will blindly boost the building's effects
- a given `Building` can be omitted safely
	- BuildingsCampaignBehavior.BuildDevelopmentsAtGameStart will go through every Town and add a building for each missing type that belongs to the settlement's type (castle/town)
		- each `Building` has a StartLevel set in DefaultBuildingTypes.InitializeAll
			- basically, 1 for fortifications, 0 for everything else
		- side note : BuildDevelopmentsAtGameStart attempts to randomly upgrade each building below level 3 based on a BuildingType's VarianceChance, but all types are initialized with a chance of 0 so the buildings will effectively be at their StartLevel
			- if this does occur, the rgl log will contain a message related to it
- ***id*** : StringId used to look up the BuildingType's object
	- see DefaultBuildingTypes.RegisterAll for the list of objects created
- ***level*** : the starting level for the building in that settlement as an integer
	- if omitted, the StartLevel for the building type is used
	- building_settlement_fortifications and building_castle_fortifications are the nodes which define what used to be the settlement's "level" attribute
		- this is now explicitly defined rather than the implicit conversion that the game was performing previously
##### `</Buildings>`
#### `</Town>`

#### `Village (SettlementComponent)`
- ***id*** : the StringId in MBObjectManager
	- generally follows the id of the parent node and is named in relation to the settlement it's bound to, eg. `village_comp_EW1_1` is the village component id for the settlement `village_EW1_1` which is in turn the first of 2 villages bound to town_EW1
- ***background_crop_position*** : parsed as a float
	- used in EncyclopediaPages and Kingdom SettlementDecisions to change the cropping for the settlement image in the VM
- ***background_mesh*** : string that is usually for creating a file name used in widgets
	- eg. EncyclopediaSettlementVM.FileName is set to `(BackgroundMeshName)_t`, and the widget's SpriteBrushLayerName is the resulting name for looking up the sprite
- ***castle_background_mesh*** : unsure where it's used
- ***wait_mesh*** : string to find the image via MenuContextHandler
	- default image for the encounter menu of the settlement
		- there's so many usages as BackgroundMesh that are unrelated to "waiting" that this is just "background_mesh" for the encounter menu in the settlement
- ***hearth*** : integer value used for the village on campaign creation
- ***village_type*** : the id is looked up in MBObjectManager and the relevant object is stored on the settlement
	- the format *must be* : `"VillageType.(type's StringId)"` as the string is split at the period :
		- `VillageType` is used to find the object type in the manager
		- the `StringId` is used to find the corresponding object within that type
	- look in DefaultVillageType.RegisterAll for the list of types
- ***bound*** : the id is looked up in MBObjectManager and the corresponding Town has the village added to its `_tradeBoundVillagesCache` on campaign creation
	- the format *must be* : `"Settlement.(settlement's StringId)"` as the string is split at the period :
		- `Settlement` is used to find the Settlement object type in the manager
		- the `StringId` is used to find the corresponding object within that type, eg. `"Settlement.castle_EN1"` finds the Settlement object for `castle_EN1` (Varagos Castle) which was previously defined
#### `</Village>`

#### Hideout (SettlementComponent)`

- ***map_icon*** : present in native xml, but seems unused
	- this is present on hideouts, eg. `map_icon="bandit_hideout_b"`, but I see nowhere that this attribute is read, stored, or fetched
- ***background_crop_position*** : parsed as a float
	- used in EncyclopediaPages and Kingdom SettlementDecisions to change the cropping for the settlement image in the VM
		- given that hideouts don't show up in either of those, this probably doesn't matter
- ***background_mesh*** : string that is usually for creating a file name used in widgets
	- eg. EncyclopediaSettlementVM.FileName is set to `(BackgroundMeshName)_t`, and the widget's SpriteBrushLayerName is the resulting name for looking up the sprite
		- given that hideouts don't show up there, this shouldn't matter
		- in particular, native hideouts have `background_mesh="empire_twn_scene_bg"` so these can be left as whatever
- ***wait_mesh*** : string to find the image via MenuContextHandler
	- default image for the encounter menu of the settlement
		- there's so many usages as BackgroundMesh that are unrelated to "waiting" that this is just "background_mesh" for the encounter menu in the settlement
- ***id*** : the StringId in MBObjectManager
	- generally identical to the id of the parent node, eg. `hideout_forest_1` is the hideout component id for the settlement `hideout_forest_1`
	- why is this the last thing in the deserialize when it's the first everywhere else?
- ***gate_rotation*** : present in native xml, but seems unused
	- this is present on hideouts, eg. `gate_rotation="6.283"`, but I see nowhere that this attribute is read, stored, or fetched, and because the entrance for hideouts is the hideout's position (because they have no defined gate_pos for X and Y), then the rotation is effectively irrelevant
#### `</Hideout>`

### `</Components>`

### `<Locations>` (`Settlement` second child node)
- ***complex_template*** : the id is looked up in MBObjectManager and the relevant template is read from to fill out the dictionary of Locations for the LocationComplex
	- the format *must be* : `"LocationComplexTemplate.(template's StringId)"` as the string is split at the period :
		- `LocationComplexTemplate` is used to find the corresponding xml
		- the `StringId` is used to find the node within the xml's template list
	- look in `location_complex_templates.xml` for the list of templates in Sandbox
- The `Location` nodes present in the settlements.xml file are only providing overrides for certain attributes that have already been read a first time from the template.
#### `<Location`
- ***id*** : used to find the existing Location with the same id that was read from the template
- ***max_prosperity*** : integer value
	- This is *actually* the maximum number of npcs that can be present in the location and is used by the AI to control the flow of npcs between locations via Passages.
- ***scene_name*** : used to define the scene that corresponds to a given upgrade level
	- Uses the \_fortifications BuildingType for Towns; default of 0 for a settlement that lacks either of those buildings.
		- Because the StartLevel for those building types is 1, and they are automatically created in Towns on campaign creation, the lowest effective level is 1 in the Town settlement type.
	- the default scene, eg. `scene_name="empire_town_g"`(the `_0` is omitted), is used as the fallback if no scene is defined for a given level
		- each scene after, eg. `scene_name_1="empire_town_g"` is used for the level corresponding to its number
			- the possibilities are `_1, _2, or _3`; the default has no number
		- it will always use the same scene name, and the SceneLevel tag is determined by looking at the Town's wall level anyways so the other entries are *probably* omittable, but untested
/>
### `</Locations>`

### `<CommonAreas>` (`Settlement` third child node)
- This is essentially a list of names that will appear on alleys in a settlement
#### <Area (CommonAreas child node)
- ***type*** : no usage, this may be a leftover from a prior version
	- alleys are tagged during load as alley\_# where the number is between 1 and the total number of areas listed in xml.
		- If no Area is present in the xml entry for a settlement, there are no alleys in it.
- ***name*** : the string is loaded into a TextObject and assigned to the alley
/>
### `</CommonAreas>`

### `</Settlement>`
