# Human-Bullet / Folded-Man problem

<center>
<video width="710" height="259" controls autoplay loop muted>
    <source src="/pics/human_bullet.webm" type="video/webm">
    Your browser does not support the video tag.
</video>
</center>



## Solution

Instead of automatically patching any method that touches Agents by calling `Harmony.PatchAll` at `OnSubModuleLoad`, you have to patch manually at a later point, 
such as `OnGameInitializationFinished` or `OnGameStart` by calling `Harmony.Patch`.

Refer to this for more details: [https://harmony.pardeike.net/articles/basics.html#manual-patching](https://harmony.pardeike.net/articles/basics.html#manual-patching)

And don't forget to call `Harmony.Unpatch` at OnGameEnd


## action_sets

!!! quote "Artem: When I added new actions via `action_sets.xslt` files this happened aswell"

Solution: just use `action_sets.xml`