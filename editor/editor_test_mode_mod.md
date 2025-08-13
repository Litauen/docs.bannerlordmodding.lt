# Editor Test Mode Mod

[Download Mod Here](https://drive.google.com/file/d/19bBsyAtn3ATjmn9c6ZA588eR8gD6seC3/view?usp=drive_link)

!!! quote "Gotha:"

Hey

First of all I created this specifically for the MP Skirmish mapping contest, but a few others mentioned that it might be interesting for other sceners as well, thats why I am posting it here too.

I wrote a small mod which basically reimplements the "Testing gamemode" into the editor.  It broke a few patches ago, so I just reimplemented the "gamemodes" (all old bugs / crashes do still exist). 
So you can basically spawn in there and walk around your map (keep in mind, this is a really minimalistic "gamemode" which should only be used to test barriers or check MP Skirmish (re)spawns.
More details below.

## How to install it?

* Ignore all virus warnings and turn off Windows Defender / Firewall if necessary to download it. The mod does not contain any harmful code 🙂 (straight into my crypto wallet)
* Extract the zip file into your Mount & Blade II Bannerlord/Modules directory
* Go into the Mount & Blade II Bannerlord/Modules/BL_AddTestScene/bin/Win64_Shipping_wEditor directory, right click the BL_AddTestScene.dll, open it's properties and check the unblock tickbox at the end of the property window (some Windows versions might not have to do this).

## How to use it?

* Start the Bannerlord Modding tool and make sure to enable the module Editor Test Mode before launching the Singleplayer
* Start the Editor
* Load a map or whatever
* Navigate to the top bar of you map and press "Start Mission as" and save your scene (or not, but keep in mind, when leaving the test mode unsaved changes will be lost!).

![](/pics/2508121532b.png)

After that you will have a small dropdown window with three available options:

SimpleMountedPlayer => The old 'default' testing mode for singleplayer/other MP gamemodes. Just spawns you as cavalry on the first Game object with the tag 'sp_play'.

and two more which were created for Skirmish:

* MPSkirmishVlandiaLightCav (spawns you as Vlandlia Light Cav)
* MPSkirmishKhuzaitHeavyArcher (spawns you as Khuzait Heavy Archer)


## Shortcuts

for all testmodes:

* O => Start a timer, press it again to stop it. Can be used to measure travel time between spawns and flags.

for Skirmish testmodes:

* U => Respawn in a random respawn zone of Team 1
* Ctrl left + U => Respawn in a random respawn zone of Team 2
* Alt left + U => Respawn in the initial spawn zone of Team 1
* Ctrl left + Alt left + U => Respawn in the initial spawn zone of Team 2
* O => Start a timer, press it again to stop it. Can be used to measure travel time between spawns and flags.
* P => Renders the contest and conquest areas around flags.

![](/pics/2508121532a.png)

## Additional features

for all "gamemodes":

* Enabled AI behaviour so you can test if horses use the correct paths to flee.
* You can place a game object with the tag 'enforce_troop_spawn' and change it's name to the class id of any class mentioned in the Mount & Blade II Bannerlord/Modules/Native/ModuleData/mpcharacters.xml you will then spawn with that class in Test mode instead. (Example: mp_heavy_cavalry_sturgia_hero or mp_light_ranged_battania_hero). Other troops might work too.
* By default the game looks for the tag 'sp_play' as respawn position. I have additionally added the 'spawnpoint_player' tag, so you can just place the 'sp_player' prefab to make things easier.  Hint: This will override the MP start spawnpoint while testing, in case you want to spawn on a specific location really quick.

![](/pics/2508121532c.png)

for MP "Skirmish":

For Skirmish just a few QoL warnings for Skirmish when starting a test mode, such as warnings for:

* No Skirmish flag placed
* Not all Skirmish flags placed
* More than one Flag A / B / C placed
* Missing spawn_visual
* Not enough soft borders
* No (re)spawn areas

![](/pics/2508121532d.png)

## Disclaimer

Keep in mind it's a mod, I just created it to make your life easier, it should not cause any crashes and when testing it on my own with a clean installed modding tool we did not encounter any issues that were caused by this tool. So don't blame me if it crashes somehow (easiest would be to just Save your progress before using the Test mode).

Another small hint: After using the Testing mode the editor will crash the next time you try to change the map to a different one until you restart your editor.

