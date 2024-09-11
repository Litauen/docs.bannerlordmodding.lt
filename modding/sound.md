# Modding sound


## Tutorials

* :material-star: [Forum post: Modding Sound & Music](https://forums.taleworlds.com/index.php?threads/modding-sound-music.433302/)
* :material-star: [Audio Modding Guide for Bannerlord](https://www.nexusmods.com/mountandblade2bannerlord/mods/1223)
* [Oficial guide - Adding Custom Sounds](https://moddocs.bannerlord.com/playing-sounds/adding-custom-sounds/)
* [Lesser Scholar Youtube Tutorial  Ep 12: Sound Effects](Link: https://youtu.be/SAONxyzDuic)
* [Bannerlord Music Modding Tutorial : Pt. 1](https://www.youtube.com/watch?v=nLGbbvoUYCI) by SixxTailsHD

## Resources

* [Free sounds](https://freesound.org/)


## Various

* [Discussion about the sound in the scenes](https://discord.com/channels/411286129317249035/697071354603765791/1101488088892579940)


## Folder structure


In ...\Modules\\\*YOUR_MOD*\

    \ModuleData\module_sounds.xml - custom definitions for our own sounds
    \ModuleData\project.mbproj - tells engine to include our module_sounds.xml
    \ModuleSounds - our audio files (.ogg, .wav)


## File format

### module_sound.xml

``` xml
<?xml version="1.0" encoding="utf-8"?>
<base xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" type="module_sound">
  <module_sounds>
    <module_sound name="YOUR_SOUND_ID" is_2d="true" sound_category="ui" path="SOUND_FILE_NAME.WAV|OGG" />
  </module_sounds>
</base>
```

#### Pitch

Can set pitch for the sound like this: min_pitch_multiplier="0.9" max_pitch_multiplier="1.1"

If values are different - random value between min-max is selected. If you need constant pitch - make both values equal.

### project.mbproj

``` xml
<?xml version="1.0" encoding="utf-8"?>
<base xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" type="solution">
  <file id="soln_module_sound" name="ModuleData/module_sounds.xml" type="module_sound" />
</base>
```

## Sound Categories

    From: \Native\ModuleData\module_sounds.xml

| Category                  | Duration   | Comment
|---------------------------|------------|---------------|
| mission_ambient_bed       | persistent | General scene ambient
| mission_ambient_3d_big    | persistent | Loud ambient sounds e.g. barn fire
| mission_ambient_3d_medium | persistent | Common ambient sounds e.g. fireplace
| mission_ambient_3d_small  | persistent | Quiet ambient sounds e.g. torch
| mission_material_impact   | max 4 sec. | Weapon clash kind of sounds
| mission_combat_trivial    | max 2 sec. | Damage extra foley like armor layer
| mission_combat            | max 8 sec. | Damage or bow release sounds
| mission_foley             | max 4 sec. | Other kind of motion fillers
| mission_voice_shout       | max 8 sec. | War cries, shouted orders, horse whinnies etc.
| mission_voice             | max 4 sec. | Grunts, exertions etc.
| mission_voice_trivial     | max 4 sec. | Small exertions like jumping
| mission_siege_loud        | max 8 sec. | Big explosions, destructions
| mission_footstep          | max 1 sec. | Human footsteps, foley
| mission_footstep_run      | max 1 sec. | Human running (contributes to BASS)
| mission_horse_gallop      | max 2 sec. | Loud mount gallop footsteps (contributes to BASS)
| mission_horse_walk        | max 1 sec. | Quiet mount footsteps, walk
| ui                        | max 4 sec. | All UI sounds
| alert                     | max 10 sec.| Pseudo-3D in-mission alerts like warning bells
| campaign_node             | persistent | Campaign map point sound like rivers etc.
| campaign_bed              | persistent | Campaign amboent bed

- Sounds that dont have valid categories wont be played!


## How to play a sound

### Read sound index

``` cs
int soundIndex = SoundEvent.GetEventIdFromString("YOUR_SOUND_ID");
```


### Several ways to play a sound

``` cs
UISoundsHelper.PlayUISound("event:/ui/inventory/take_all");
```

Worked ok:

``` cs
mission.MakeSound(soundIndex, Vec3.Zero, false, true, -1, -1);
```

Did not work:

``` cs
SoundEvent.PlaySound2D(soundIndex);
```

Works:

``` cs
SoundEvent eventRef = SoundEvent.CreateEvent(soundIndex, mission.Scene);
eventRef.SetPosition(_mission.MainAgent.Position);
eventRef.Play();
```

## is2D option

Setting is2D to true makes it play at a constant volume throughout the entire scene.

is_2d="true" in XML

## Sound in menus

```cs
args.MenuContext.SetPanelSound("event:/ui/panels/settlement_village");  // one-time sound on opening
args.MenuContext.SetAmbientSound("event:/map/ambient/node/settlements/2d/village");
```

!!! question "Why does not work on wait-type menus?"


## Sound Events

!!! quote "hunharibo: You don't need the event prefix. Only vanilla sounds use 'event:' to identify sounds in the fmod middleware. Since fmod is a licensed proprietary library, modders cant use that"

Possible to play sound (events) with InformationManager.AddQuickInformation(text, 0, null, <span style="color:green">string SoundEventPath</style>)

You need to null the SoundEvent when you leave the mission.

Same with MultiSelectionInquiryData and in the menus

In GauntletUI: HandleQueryCreated played with SoundEvent.PlaySound2D(soundEventPath);

``` cs
SoundEvent.PlaySound2D("event:/ui/notification/quest_fail");
```

soundEventPath looks like:


    "event:/ui/notification/levelup"


Defined in [GAME_FOLDER]\Sounds\GUIDs.txt

??? example "event:/ui list"

    event:/ui/campaign/autobattle_defeat<br>
    event:/ui/campaign/autobattle_large<br>
    event:/ui/campaign/autobattle_small<br>
    event:/ui/campaign/click_party<br>
    event:/ui/campaign/click_party_enemy<br>
    event:/ui/campaign/click_settlement<br>
    event:/ui/campaign/click_settlement_enemy<br>
    event:/ui/campaign/focus<br>
    event:/ui/checkbox<br>
    event:/ui/choice<br>
    event:/ui/conversation<br>
    event:/ui/crafting/craft_success<br>
    event:/ui/crafting/craft_tab<br>
    event:/ui/crafting/randomize<br>
    event:/ui/crafting/refine_success<br>
    event:/ui/crafting/refine_tab<br>
    event:/ui/crafting/smelt_success<br>
    event:/ui/crafting/smelt_tab<br>
    event:/ui/cutscenes/child_coming_of_age<br>
    event:/ui/cutscenes/child_newborn<br>
    event:/ui/cutscenes/child_newborn_mother<br>
    event:/ui/cutscenes/child_newborn_orphan<br>
    event:/ui/cutscenes/death_clan<br>
    event:/ui/cutscenes/death_clan_war<br>
    event:/ui/cutscenes/death_old_age<br>
    event:/ui/cutscenes/death_war_defeat<br>
    event:/ui/cutscenes/death_war_victory<br>
    event:/ui/cutscenes/execution<br>
    event:/ui/cutscenes/execution_female<br>
    event:/ui/cutscenes/kingdom_created<br>
    event:/ui/cutscenes/kingdom_destroyed<br>
    event:/ui/cutscenes/kingdom_join<br>
    event:/ui/cutscenes/marriage<br>
    event:/ui/cutscenes/story_banner_assemble_part_01<br>
    event:/ui/cutscenes/story_banner_assemble_part_02<br>
    event:/ui/cutscenes/story_banner_find_01<br>
    event:/ui/cutscenes/story_banner_find_02<br>
    event:/ui/cutscenes/story_become_king<br>
    event:/ui/cutscenes/story_claim<br>
    event:/ui/cutscenes/story_conspiracy_activate<br>
    event:/ui/cutscenes/story_conspiracy_start<br>
    event:/ui/cutscenes/story_defeat<br>
    event:/ui/cutscenes/story_empire_destroyed<br>
    event:/ui/cutscenes/story_empire_united<br>
    event:/ui/cutscenes/story_pledge_support<br>
    event:/ui/cutscenes/story_pledge_support_nonempire<br>
    event:/ui/default<br>
    event:/ui/dropdown<br>
    event:/ui/empty<br>
    event:/ui/endgame/end_clan_destroyed<br>
    event:/ui/endgame/end_retirement<br>
    event:/ui/endgame/end_victory<br>
    event:/ui/endgame/tab_battles<br>
    event:/ui/endgame/tab_companions<br>
    event:/ui/endgame/tab_crafting<br>
    event:/ui/endgame/tab_finances<br>
    event:/ui/endgame/tab_general<br>
    event:/ui/filter<br>
    event:/ui/inspect<br>
    event:/ui/inventory/animal<br>
    event:/ui/inventory/animal_butcher<br>
    event:/ui/inventory/bow<br>
    event:/ui/inventory/crossbow<br>
    event:/ui/inventory/helmet<br>
    event:/ui/inventory/horse<br>
    event:/ui/inventory/horsearmor<br>
    event:/ui/inventory/leather<br>
    event:/ui/inventory/leather_lite<br>
    event:/ui/inventory/onehanded<br>
    event:/ui/inventory/perk<br>
    event:/ui/inventory/pickup<br>
    event:/ui/inventory/polearm<br>
    event:/ui/inventory/quiver<br>
    event:/ui/inventory/sack<br>
    event:/ui/inventory/shield<br>
    event:/ui/inventory/take_all<br>
    event:/ui/inventory/throwing<br>
    event:/ui/inventory/twohanded<br>
    event:/ui/inventory/unit<br>
    event:/ui/item_close<br>
    event:/ui/item_open<br>
    event:/ui/mini_popup<br>
    event:/ui/mission/arena_victory<br>
    event:/ui/mission/ballista<br>
    event:/ui/mission/batteringram<br>
    event:/ui/mission/catapult<br>
    event:/ui/mission/deploy<br>
    event:/ui/mission/drinks<br>
    event:/ui/mission/emptyslot<br>
    event:/ui/mission/horns/attack<br>
    event:/ui/mission/horns/move<br>
    event:/ui/mission/horns/retreat<br>
    event:/ui/mission/multiplayer/changeclass<br>
    event:/ui/mission/multiplayer/defeat<br>
    event:/ui/mission/multiplayer/gamestart<br>
    event:/ui/mission/multiplayer/lastmanstanding<br>
    event:/ui/mission/multiplayer/pointcapture<br>
    event:/ui/mission/multiplayer/pointlost<br>
    event:/ui/mission/multiplayer/pointsremoved<br>
    event:/ui/mission/multiplayer/pointwarning<br>
    event:/ui/mission/multiplayer/roundstart<br>
    event:/ui/mission/multiplayer/victory<br>
    event:/ui/mission/siege_engine<br>
    event:/ui/mission/siegetower<br>
    event:/ui/multiplayer/click_class<br>
    event:/ui/multiplayer/click_culture<br>
    event:/ui/multiplayer/click_item<br>
    event:/ui/multiplayer/click_purchase<br>
    event:/ui/multiplayer/coin_add<br>
    event:/ui/multiplayer/levelup<br>
    event:/ui/multiplayer/match_ready<br>
    event:/ui/multiplayer/pick_sigil<br>
    event:/ui/multiplayer/shop_purchase<br>
    event:/ui/multiplayer/shop_purchase_complete<br>
    event:/ui/multiplayer/shop_purchase_proceed<br>
    event:/ui/multiplayer/wear_armor_big<br>
    event:/ui/multiplayer/wear_armor_small<br>
    event:/ui/multiplayer/wear_generic<br>
    event:/ui/multiplayer/wear_helmet<br>
    event:/ui/multiplayer/xpbar<br>
    event:/ui/multiplayer/xpbar_stop<br>
    event:/ui/notification/alert<br>
    event:/ui/notification/army_created<br>
    event:/ui/notification/army_dispersion<br>
    event:/ui/notification/child_born<br>
    event:/ui/notification/coins_negative<br>
    event:/ui/notification/coins_positive<br>
    event:/ui/notification/death<br>
    event:/ui/notification/education<br>
    event:/ui/notification/hideout_found<br>
    event:/ui/notification/kingdom_decision<br>
    event:/ui/notification/levelup<br>
    event:/ui/notification/marriage<br>
    event:/ui/notification/peace<br>
    event:/ui/notification/peace_offer<br>
    event:/ui/notification/quest_fail<br>
    event:/ui/notification/quest_finished<br>
    event:/ui/notification/quest_start<br>
    event:/ui/notification/quest_update<br>
    event:/ui/notification/ransom_offer<br>
    event:/ui/notification/relation<br>
    event:/ui/notification/settlement_owner_change<br>
    event:/ui/notification/settlement_rebellion<br>
    event:/ui/notification/settlement_under_siege<br>
    event:/ui/notification/tournament_end<br>
    event:/ui/notification/trait_change<br>
    event:/ui/notification/truce<br>
    event:/ui/notification/unlock<br>
    event:/ui/notification/war_declared<br>
    event:/ui/oob/dropdown<br>
    event:/ui/oob/formation_archers<br>
    event:/ui/oob/formation_cavalry<br>
    event:/ui/oob/formation_cavalry_horse_archers<br>
    event:/ui/oob/formation_horse_archers<br>
    event:/ui/oob/formation_infantry<br>
    event:/ui/oob/formation_infantry_archers<br>
    event:/ui/oob/officer_drag<br>
    event:/ui/oob/officer_pick<br>
    event:/ui/panels/battle/attack_large<br>
    event:/ui/panels/battle/attack_medium<br>
    event:/ui/panels/battle/attack_small<br>
    event:/ui/panels/battle/encounter_attacker<br>
    event:/ui/panels/battle/encounter_defender<br>
    event:/ui/panels/battle/retreat<br>
    event:/ui/panels/battle/slide_in<br>
    event:/ui/panels/encyclopedia_open<br>
    event:/ui/panels/encyclopedia_page<br>
    event:/ui/panels/next<br>
    event:/ui/panels/panel_character_open<br>
    event:/ui/panels/panel_clan_open<br>
    event:/ui/panels/panel_inventory_open<br>
    event:/ui/panels/panel_kingdom_open<br>
    event:/ui/panels/panel_party_open<br>
    event:/ui/panels/panel_quest_open<br>
    event:/ui/panels/panel_trading_loop<br>
    event:/ui/panels/perk<br>
    event:/ui/panels/previous<br>
    event:/ui/panels/recruit<br>
    event:/ui/panels/scoreboard_flags<br>
    event:/ui/panels/scoreboard_skill<br>
    event:/ui/panels/scoreboard_tournament<br>
    event:/ui/panels/settlement_city<br>
    event:/ui/panels/settlement_hideout<br>
    event:/ui/panels/settlement_keep<br>
    event:/ui/panels/settlement_village<br>
    event:/ui/panels/siege/besiege<br>
    event:/ui/panels/siege/engine_build<br>
    event:/ui/panels/siege/engine_build_complete<br>
    event:/ui/panels/siege/engine_click<br>
    event:/ui/panels/siege/lead_assault<br>
    event:/ui/panels/siege/raid<br>
    event:/ui/panels/siege/sally_out<br>
    event:/ui/panels/skill_add<br>
    event:/ui/panels/skill_new<br>
    event:/ui/panels/tutorial<br>
    event:/ui/panels/twopanel_open<br>
    event:/ui/panels/upgrade<br>
    event:/ui/party/recruit prisoner<br>
    event:/ui/party/recruit_all<br>
    event:/ui/party/upgrade<br>
    event:/ui/persuasion/critical_fail<br>
    event:/ui/persuasion/critical_success<br>
    event:/ui/persuasion/ineffective<br>
    event:/ui/persuasion/success<br>
    event:/ui/popup<br>
    event:/ui/reign/decision<br>
    event:/ui/reign/vote<br>
    event:/ui/sort<br>
    event:/ui/stealth/stealth_notification<br>
    event:/ui/tab<br>
    event:/ui/transfer<br>



??? info "Some [info](https://discord.com/channels/411286129317249035/697071354603765791/1096586744301887629) about events"
    * the event:/ derives from the FMOD event paths that the game uses to play sound events
    * however when we mod the game’s audio we don’t have access to the FMOD project and their events, so we have to replace entire events with singular oneshots
    * the event:/ is part of an event path
    * for example, “event:/Ambience/Rain” would be an event called Rain located in a folder called Ambience


??? abstract "Sound examples"

    ui/mission sounds

    <center>
        <iframe width="640" height="360" src="https://www.youtube.com/embed/OW0NNq6bozQ" frameborder="0" allowfullscreen></iframe>
    </center>

    ui/multiplayer sounds

    <center>
        <iframe width="640" height="360" src="https://www.youtube.com/embed/uEmcsDaKYkc" frameborder="0" allowfullscreen></iframe>
    </center>

    ui/notification sounds

    <center>
        <iframe width="640" height="360" src="https://www.youtube.com/embed/Je7XWMkqwTE" frameborder="0" allowfullscreen></iframe>
    </center>

    alerts sounds

    <center>
        <iframe width="640" height="360" src="https://www.youtube.com/embed/-SibjXyxMsA" frameborder="0" allowfullscreen></iframe>
    </center>


## Issues with sound volume using SoundEvent

??? quote "Windwhistle: My findings so far:"

    When using SoundEvent to play a custom sound in module_sounds.xml from the sound category "mission_ambient_3d_small" and "mission_ambient_3d_medium", if the attribute is_2d is set to false, the volume of the sound at max volume (close to the sound source) is directly related to the distance at which it was spawned to the player.

    So, if you set the position of the SoundEvent FARTHER from the player, it will be FAINTER at MAX volume. If you set the position of the SoundEvent CLOSER to the player, it will be LOUDER at MAX volume.

    It draws confusion, as I do not mean that the sound becomes fainter as you travel farther away from it. That's normal. I am not talking about that. There's some videos if you scroll up that depict what I am talking about, because it's pretty hard to describe.

    To fix this: I just set is_2d to true instead of false, and the sound plays how I wanted it to play.

    Why: I don't know. I'm going to test it out in a regular scene/mission to see if it's my code somehow, or if it's a genuine bug. It might not even be a bug but intentional, but it'd be weird if it were.

    EDIT: it also suffers from the same problem in a regular scene. Setting the position repeatedly on tick also does not alleviate the issue. Making the file format .wav also did not help.

    [Source](https://discord.com/channels/411286129317249035/677511186295685150/1123190415420555344){target=_blank}

## Delayed stop to prevent ambient sounds from looping

``` cs
private async void DelayedStop(SoundEvent eventRef)
{
  await Task.Delay(soundDuration);
  eventRef.Stop();
}
```

!!! question "How to get sound duration?"

??? info "Stop looping example"

    ``` cs
    SoundEvent eventRef = SoundEvent.CreateEvent(SoundEvent.GetEventIdFromString(SoundEffectOnUse), Scene);
    eventRef.SetPosition(GameEntity.GetGlobalFrame().origin);
    eventRef.Play();
    DelayedStop(eventRef); // used to prevent ambient sounds from looping
    ```

## Short sounds for agents in Mission

``` cs
agent.MakeVoice(SkinVoiceManager.VoiceType.Grunt, SkinVoiceManager.CombatVoiceNetworkPredictionType.NoPrediction);
```

??? abstract "SkinVoiceManager.VoiceType examples"
    - Grunt - eeh
    - Jump - yah
    - Yell - eyah, hey, yeee
    - Pain - uh, mhmph
    - Death - uuuuhmmm
    - Stun - yyy, aaaa, eeee
    - Fear - yaaaaah, aaaah
    - Climb - ?
    - Focus - (silence?)
    - Debacle - aaaaaaah, shriek/screech
    - and many others...


## Custom Music

Create your own soundtrack.xml in YOURMOD/music/soundtrack.xml

``` cs
using System;
using HarmonyLib;
using psai.net;
using TaleWorlds.Engine;
using TaleWorlds.MountAndBlade;
using TaleWorlds.ModuleManager;

namespace Patches
{
    [HarmonyPatch]
    public static class MBMusicManagerPatches
    {
        [HarmonyPrefix]
        [HarmonyPatch(typeof(MBMusicManager), "Initialize")]
        public static void OverrideCreation()
        {
            if (!NativeConfig.DisableSound)
            {
                string corePath = ModuleHelper.GetModuleFullPath("YOURMOD") + "music/soundtrack.xml";
                PsaiCore.Instance.LoadSoundtrackFromProjectFile(corePath);
            }
        }

        // this will only work if you have custom audio? with ID 401. By default comment this out to play default menu music
        [HarmonyPrefix]
        [HarmonyPatch(typeof(MBMusicManager), "ActivateMenuMode")]
        public static bool UseTowMenuMusicId(ref MBMusicManager __instance)
        {
            Random rnd = new Random();
            int index = rnd.Next(401, 401);
            typeof(MBMusicManager).GetProperty("CurrentMode").SetValue(__instance, MusicMode.Menu);
            PsaiCore.Instance.MenuModeEnter(index, 0.5f);
            return false;
        }
    }
}
```

In your MOD/music/soundtrack.xml you can reference native audio by adding ..\\..\\..\\..\music\PC\ in their &lt;Path>

Original:

    <Path>MBII_Campaign_Maintheme_Variation.wav</Path>

After the patch:

    <Path>..\..\..\..\music\PC\MBII_Campaign_Maintheme_Variation.wav</Path>

## Custom Music in Taverns

By culture:

    Sandbox\ModuleData\settlement_tracks.xml



## Custom Music in Main Menu

Source [here](https://www.nexusmods.com/mountandblade2bannerlord/mods/5233)

1. Find the music
2. Convert to OGG
3. In \Mount & Blade II Bannerlord\music\Soundtrack.XML find Maintheme and under it: &lt;TotalLengthInSamples>
4. Change the number to your sample length using:

    ![](/pics/2402262117.png)

    or multiply 44305 X {Your song length in seconds}

5. Backup \Mount & Blade II Bannerlord\music\PC\Maintheme.ogg
6. Rename your music ogg file to Maintheme.ogg and place it in \Mount & Blade II Bannerlord\music\PC\


## Custom Music in Culture selection menu

Requires patching:

``` cs
public class CharacterCreationCultureStageView : CharacterCreationStageViewBase
{
.....
  private void OnCultureSelected(CultureObject culture)
  {
      MissionSoundParametersView.SoundParameterMissionCulture soundParameterMissionCulture = MissionSoundParametersView.SoundParameterMissionCulture.None;
      if (culture.StringId == "aserai")
      {
          soundParameterMissionCulture = MissionSoundParametersView.SoundParameterMissionCulture.Aserai;
      }
      else if (culture.StringId == "khuzait")
      {
          soundParameterMissionCulture = MissionSoundParametersView.SoundParameterMissionCulture.Khuzait;
      }
      else if (culture.StringId == "vlandia")
      {
          soundParameterMissionCulture = MissionSoundParametersView.SoundParameterMissionCulture.Vlandia;
      }
      else if (culture.StringId == "sturgia")
      {
          soundParameterMissionCulture = MissionSoundParametersView.SoundParameterMissionCulture.Sturgia;
      }
      else if (culture.StringId == "battania")
      {
          soundParameterMissionCulture = MissionSoundParametersView.SoundParameterMissionCulture.Battania;
      }
      else if (culture.StringId == "empire")
      {
          soundParameterMissionCulture = MissionSoundParametersView.SoundParameterMissionCulture.Empire;
      }

      SoundManager.SetGlobalParameter("MissionCulture",(float)soundParameterMissionCulture);
  }
...
}
```

More info by [-Mr. E-](https://discord.com/channels/411286129317249035/677511186295685150/1259676285756641401)


## Game Sound Settings

Get/set Sound/Music Volume:

``` cs
NativeOptions.GetConfig(NativeOptions.NativeOptionsType.SoundVolume)
NativeOptions.GetConfig(NativeOptions.NativeOptionsType.MusicVolume)
NativeOptions.SetConfig(NativeOptions.NativeOptionsType.SoundVolume, _soundVolume);
NativeOptions.SetConfig(NativeOptions.NativeOptionsType.MusicVolume, _musicVolume);
```

## Manage Game Music

Stop the music with fade-out:

``` cs
PsaiCore.Instance.StopMusic(true, 1.5f);
```

Continue playing the music:

``` cs
PsaiCore.Instance.ReturnToLastBasicMood(true);
```

## Extract native sounds

Use [Fmod Bank Tools](https://www.nexusmods.com/rugbyleaguelive3/mods/2?tab=docs)

![](/pics/2409030815.png)

## NAudio

!!! quote "Artem:"
    This is for everyone who tries to add sounds to the game and is annoyed by the game restrictions, like sounds being too short, too quiet, etc.

[https://github.com/naudio/NAudio](https://github.com/naudio/NAudio){target=_blank}


??? tip "Install NAudio"

    RMB on your project:<br>
    ![](/pics/2404110856a.jpg)

    ![](/pics/2404110856b.jpg)

    ![](/pics/2404110856c.jpg)

    ![](/pics/2404110856d.jpg)




??? example "Example Play Audio method"
    ``` cs

    using NAudio.Wave;

    public static async void PlayNAudioSound(string audioFileName, double soundLevel = 1.0, double musicLevel = 1.0, bool soundLevelByNativeMusic = false, bool fadeOutNativeMusic = false)
    {

        if (audioFileName.Length == 0) return;

        string _audioLocation;
        AudioFileReader _audioFile;
        WaveOutEvent _outputDevice = _outputDevice = new WaveOutEvent();

        string fullName = Directory.GetParent(Directory.GetParent(Directory.GetParent("YOUR_MODULE").FullName).FullName).FullName;
        _audioLocation = System.IO.Path.Combine(fullName, "Modules\\YOUR_MODULE\\ModuleSounds\\General\\");
        try
        {

            _audioFile = new AudioFileReader(_audioLocation + audioFileName + ".wav");

            // set sound level by native sound or music level
            if (soundLevelByNativeMusic)
            {
                _audioFile.Volume = NativeOptions.GetConfig(NativeOptions.NativeOptionsType.MusicVolume) * (float)musicLevel;
            }
            else
            {
                _audioFile.Volume = NativeOptions.GetConfig(NativeOptions.NativeOptionsType.SoundVolume) * (float)soundLevel;
            }

            // take into account Master Volume level
            _audioFile.Volume = _audioFile.Volume * NativeOptions.GetConfig(NativeOptions.NativeOptionsType.MasterVolume);

            _outputDevice.Init(_audioFile);

            if (fadeOutNativeMusic) PsaiCore.Instance.StopMusic(true, 1.5f); // stop native Music with fade-out

            _audioFile.Seek(0L, SeekOrigin.Begin);
            _outputDevice.Play();

            // calculate the duration of the audio
            // Get the total number of samples in the file
            long totalSamples = _audioFile.Length / (_audioFile.WaveFormat.BitsPerSample / 8 * _audioFile.WaveFormat.Channels);
            // Calculate the total duration in milliseconds
            double durationMs = (double)totalSamples / _audioFile.WaveFormat.SampleRate * 1000;


            await Task.Delay((int)durationMs);  // wait till audio stops playing

            if (fadeOutNativeMusic) PsaiCore.Instance.ReturnToLastBasicMood(true); // restore native Music

            // cleanup
            if (_outputDevice != null)
            {
                _outputDevice.Stop();
                _outputDevice.Dispose();
            }

        }
        catch
        {
                // ERROR   ($"ERROR: can't play NAudio sound: {audioFileName}. Missing folders/files? " + _audioLocation);
        }

    }
    ```

Implementation example: [Artem's Assassination Mod](https://www.nexusmods.com/mountandblade2bannerlord/mods/5653){target=_blank}



!!! quote "hunharibo: I expanded on the NAudio integration idea a lot in TOR. Made a MixingSampleProvider so you can actually play multiple sound sources at the same time if you wanted to. It only takes 48khz stereo vorbis files though. [LINK](https://github.com/TheOldRealms/TOR_Core/tree/development/CSharpSourceCode/Audio). Also has rudimentary 3d spatial sound with stereo panning and distance attenuation"