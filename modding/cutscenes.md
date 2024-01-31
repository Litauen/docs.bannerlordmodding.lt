# Cutscenes

![](/pics/NBh2a8Y.png)<br>
We are waiting... (No pressure :grin:)

All info in this page is wisdom nuggets from Artem (picked over from the Discord).

## Location

    Steam\steamapps\common\Mount & Blade II Bannerlord\Modules\SandBox\SceneObj

the folders that start with scn_cutscene

## Editing

You can edit the cutscenes just like any other scene, be sure to not remove any characters or camera stuff related to it, otherwise the cutscene will crash your game, it's very brittle and you have to be very careful not to delete anything that will be read by bannerlords code.

The technical aspect of working for a cutscene is different than normal scene, in the cutscene there are special agents called cutscene_agent which will be called by code. They have animation instances for initial start of the scene, positive outcome, negative outcome.


## Animation instances

You can find animation instances inside Native\ModuleData\action_sets the ones that start with act.


## Other

If you're creating a cutscene you need to determine a scene notification item class, if you just add npcs to your scene they won't show up inside the cutscene since they need to have a special cutscene tag which is determined inside your scene notification code, the amount of people, the banner, the text for the cutscene, everything except the scene itself is inside code 

These classes should give you a good understanding:

