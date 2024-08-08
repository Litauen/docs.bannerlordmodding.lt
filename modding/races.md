# Races

## Create a new race

Info by [TheCooler Jason](https://discord.com/channels/411286129317249035/697071354603765791/1269438122278653973)

step 1: copy/paste your monsters.xml and skins.xml from native folder

step 2: open the new skins.xml file, copy/paste everything five times except for the <skins> and </skins> brackets at the top and bottom of the file

step 3: press control + f and search for id="human"; for all six replace them with id="aserairace", id="sturgianrace", id="battanianrace", etc

step 4: add skins.xml to your project.mbproj file

step 5: open the new monsters.xml file, delete all the monsters aside from id="human" one; then copy/paste that monster 5 times

step 6: press control + f and search for id="human"; for all six replace them with id="aserairace", id="sturgianrace", id="battanianrace", etc

step 7: add monsters.xml to your submodule

then also adding a race="aserairace" to each troop and npc from aserai culture which will probably take a few hours

race support was added several years into bannerlord's development, so by default everything is human

you add a tag like this to the NPCCharacter:

![](/pics/2408071033.png)

