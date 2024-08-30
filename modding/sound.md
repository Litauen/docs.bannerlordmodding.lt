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
args.MenuContext.SetPanelSound("event:/ui/panels/settlement_village");
args.MenuContext.SetAmbientSound("event:/map/ambient/node/settlements/2d/village");
```

## Sound Events

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

??? example "soundEventPath examples"

    event:/ui/notification/army_created<br>
    event:/ui/notification/army_dispersion<br>
    event:/ui/notification/child_born<br>
    event:/ui/notification/coins_negative<br>
    event:/ui/notification/coins_positive<br>
    event:/ui/notification/death<br>
    event:/ui/notification/hideout_found<br>
    event:/ui/notification/kingdom_decision<br>
    event:/ui/notification/levelup<br>
    event:/ui/notification/marriage<br>
    event:/ui/notification/peace<br>
    event:/ui/notification/quest_fail<br>
    event:/ui/notification/quest_finished<br>
    event:/ui/notification/quest_start<br>
    event:/ui/notification/quest_update<br>
    event:/ui/notification/relation<br>

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

## NAudio

??? tip "Install NAudio"

    RMB on your project:<br>
    ![](/pics/2404110856a.jpg)

    ![](/pics/2404110856b.jpg)

    ![](/pics/2404110856c.jpg)

    ![](/pics/2404110856d.jpg)

!!! quote
    Artem: For everyone that tries to add sounds to the game and is annoyed by the games restrictions, like sounds being too short, too quiet etc.

[https://github.com/naudio/NAudio](https://github.com/naudio/NAudio){target=_blank}

``` cs
using NAudio.Wave;

private AudioFileReader audioFile;

public static async void PlayCustomSound()
{

    if (this.outputDevice == null) this.outputDevice = new WaveOutEvent();

    try
    {
        //string assemblyFolder = Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
        string fullName = Directory.GetParent(Directory.GetParent(Directory.GetParent(Assembly.GetExecutingAssembly().Location).FullName).FullName).FullName;
        this.audioLocation = System.IO.Path.Combine(fullName, "ModuleSounds\\");
        this.audioFile = new AudioFileReader(this.audioLocation + "encounter_river_pirates_9.wav");
        this.audioFile.Volume = NativeOptions.GetConfig(NativeOptions.NativeOptionsType.SoundVolume);
        this.outputDevice.Init(this.audioFile);

        PsaiCore.Instance.StopMusic(true, 1.5f); // stop native Music with fade-out

        this.audioFile.Seek(0L, SeekOrigin.Begin);
        this.outputDevice.Play();

        // restore native Music
        await Task.Delay(4500); // change 4500 to the length of your sound file in ms
        PsaiCore.Instance.ReturnToLastBasicMood(true);

    }
    catch {
        // "ERROR: can't play custom sound. Missing folders/files? "
    }
}

// stop the sound
if (outputDevice != null)
{
    outputDevice.Stop();
    outputDevice.Dispose();
}

```

Implementation example: [Artem's Assassination Mod](https://www.nexusmods.com/mountandblade2bannerlord/mods/5653){target=_blank}



!!! quote "hunharibo: I expanded on the NAudio integration idea a lot in TOR. Made a MixingSampleProvider so you can actually play multiple sound sources at the same time if you wanted to. It only takes 48khz stereo vorbis files though. [LINK](https://github.com/TheOldRealms/TOR_Core/tree/development/CSharpSourceCode/Audio). Also has rudimentary 3d spatial sound with stereo panning and distance attenuation"