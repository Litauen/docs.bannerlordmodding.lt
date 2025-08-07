# Custom Mount

!!! quote "Artem:"

To implemement custom mount you'd need:

* action_sets.xml (for mount animations)
* action_sets.xslt (for human animations for the new mount)
* action_types.xslt (to reference the animations and to add types of animations (example actt_idle, actt_hit_object, etc)
* monsters.xml (referencing bones, speed, weight and other important stuff)
* items.xml (mounts are just same as items so you need to add them as a item aswell)
* monster_usage.xml (you need to create a new usage set for your new mount if not using horse skeleton)
* monster_usage_sets.xslt (needed to add mounting, dismounting, falling animations (otherwise it would crash))

and tinker with the skeleton editor inside modding tools.
