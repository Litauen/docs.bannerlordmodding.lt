# Siege Scenes

- [Official documentation](https://moddocs.bannerlord.com/authoring-mission-scenes/sieges/)


!!! info "This text is made by AI from the chat by Bullero, Reynauld, Hart, Le Profyteur, Snorri, JOJABO"

## Level Names

- Level_1
- Level_2
- Level_3
- Siege
- Civilian
- Sally
- Base

---

## Level Visibility

- Base off
- Lvl 1-3 on
- Siege on
- Civilian off
- Sally on




- Siege equipment deployment points need to have certain lvls of visibility or else it can result in a crash.

---

## Navmesh

- Navmesh id need to be set to id:1 for inside settlement and wall.
- Navmesh id need to be set to id:0 for outside.
- If you don't make this, you can't deploy troops.
- There is also a issue currently with navmesh not connecting well if its in a certain angle.
- If navmesh goes through the ladder, there is an issue.
- If navmesh is too close to the ladder, it takes the ladder path again.
- Going a little higher with navmesh than the ladder works better.
- AI in mass and fight will treat navmesh as a tip, not railroad.
- AI will fall off a lot from places, especially at angles and in kitbashed structures.
- Barrier or way outside under every non solid stairways.

---

## Ladders

- If troops stuck on top of a ladder, you need to rearrange it.
- I have no clue why they get stuck sometimes.
- If troops can't climb siege tower / stuck under it / stuck on ladders, siege tower is positioned at too big angle for ladders.
- Ladders should be pretty much flat when facing walls.
- Sometimes move vertex few cm on one side or another can fix it.
- Once it works first try, other time you need to fix it countless times.

---

## Siege Towers

- If troops don't use siege tower after it drop ramp, rearrange its navmesh matching place on walls.
- Sometimes move vertex few cm can fix it.
- Other time you need to fix it countless times.

---

## Siege Engines

- Trebuchets got lowest range from all siege engines.
- You want to line up other attacker ones matching to its range.
- Defender mangonels are very friendly fire friendly.
- AI can't see enemy behind wall.
- AI won't shoot on something it don't see.
- AI will see enemy behind allied troops and shoot them hitting allies first.
- Ballistas don't have this problem.
- Ballistas are best siege engines TW did.
- Ballistas are smartly made script/mesh wise.
- Ballistas got best AI.
- You need to place somewhere 4 mangonels in castle that will have clear sight on enemy.
- Breach filler script is somewhere in siege engines scripts.
- Breach filler doesn't work well.
- AI forget about breach filler in half cases.
- It's mentioned in TW documentation.
- It's not used in any vanilla scene.


!!! quote "Snorri - from extensive siege scenes testing from creating them and figguring out how to make AI stupidnes at leest mimic what should be done"

* Siege engines mostly aim for enemy siege engines (or rather siege engine crew), so anything around them could also by targeted by mistake. Placing your troops next to siege engines is not a best idea.
* It seems that after extensive, non effectice fire to enemy siege engines, or if they go beyond reach, or if there any from start, it choose another target.
* Siege engines can aim into singular troops, and it's super weird how precise it could be. Usually they will chose targets that are closer to them, but not always, it dose't seems to me that AI know if it shoot to whole formation or singular troop, so it will not chose to shoot to bigger stack as would be logical for most kills ratio.
* Siege engines need more or less direct sight to target, they can't shoot over walls, hills etc. it also don't care if friendly troops are in a way, that's why most defender siege engines are places nearly always so much on front, they can do sooo much friendly fire if you will give them a chance (as they will chose to shot more nearby enemy that is probably in fight with your troops).
* Siege engines don't seems to care about destructible things as merlons and roofs, they destroy them by mistake trying to target troops/siege engines.



---

## Walls / Merlons / HP

- Vanilla merlons are absolutely bad.
- They don't cover troops on top of wall.
- Lower parts need to cover them good.
- Otherwise attacker will have better archer positions.
- Wooden stick barricades attackers use got 200 HP.
- Vanilla stone/trunk/stucco ones have always 1.
- 1 HP because of `_destruciblebystoneonly` tag.
- Pierce damage does very low damage against entities.
- Wooden barricade > stone merlon.
- One round of well placed grapeshot can obliterate wall defence.
- Same thing maybe destroy one barricade in same time.
- Some increase HP of higher level ones.

---

## Invisible Walls

- Invisible walls are needed for many parts of walls, stairs, tight places.
- AI will fall where they supposed not to.
- Barrier under every non solid stairways.

---

## Camera

- Watch out for camera and zones.
- Camera is hard requirement.
- You can place high limiter if you got cave siege.
- Bannerlord cameras like to sometimes vanish or move after saving.
- Indoor sieges camera flies through roof.

---

## Scene Build Approach

- Start very simple.
- Flat map.
- 4 towers.
- Wall on each side.
- One outer door.
- One inner door.
- Make paths for siege towers and ram.
- Put ladders.
- Make navmesh.
- Make scripts and tags.
- Place siege engine for both side.
- Add map to custom battle and test.
- Test with only ladder.
- Add ram and retry.
- Add towers and retry.
- Add siege engines one by one.
- Finish with breach because navmesh is annoying to make.
- Anything more complex adds to Bannerlord AI and engine issues.
- No amount of testing for more complex layered custom multi lvl sieges that one man can do will find all issues.
- DNspy will find easy bugs.
- Use debugger and better exception windows.
- Keep everything clean.
- Don't delete some destructible scripted prefab part.
- You need patience and a lot of coffee.

---

## Other

- Don't forget arrow barrels.
- Sally mission.
- Civilian off.
- Siege on.
- Level 1-3 on.
- Base off.
