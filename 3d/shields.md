# Shields

* [Bannerlord 3D Asset Workflow](https://docs.google.com/document/d/1aHBsO3mzkT0JsbCt9aCOh6CWAFATXwKtSaVb__TYIoo/edit)


## Meshes

Shields require 3 meshes/models for full implementation: the mesh for visuals, and two physics meshes.

![](/pics/2409281124.png)
<p style="font-size:12px;">Note: they are moved to the sides for a preview, when putting into the game they should be aligned in the same space.</p>


* bo_cap_SHIELDNAME (bo_cap_lt_heater_shield in the example) is likely the ragdoll hitbox for the shield
* bo_SHIELDNAME (bo_lt_heater_shield in the example) likely is the hitbox for projectiles and melee
* SHIELDNAME (lt_heater_shield in the example) - the model that is going to be seen by the player



## Materials

![](/pics/2409282003.png)

These physics meshes require their material to be named in the following way:

* bo_cap_SHIELDNAME needs its material to be called “wood”
* bo_SHIELD needs a material “wood_shield”
* SHIELDNAME material can have the same name: SHIELDNAME

Renaming is done in your 3D program, when imported to the Editor, it’ll automatically assign the material you have named. 

!!! tip "Make sure you create a matching material in the Editor _before_ you import your FBX. If not - you will get many errors and will have to assign material to all the LODs by hand."




<br><br>
## Clan Sigil

![](/pics/2409281117.png)
