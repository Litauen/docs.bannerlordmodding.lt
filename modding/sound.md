# Modding sound


## Tutorials

* :material-star: [Forum post: Modding Sound & Music](https://forums.taleworlds.com/index.php?threads/modding-sound-music.433302/)
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

### project.mbproj

``` xml
<?xml version="1.0" encoding="utf-8"?>
<base xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" type="solution">
  <file id="soln_module_sound" name="ModuleData/module_sounds.xml" type="module_sound" />
</base>
```


## How to play a sound

### Read sound index

``` cs
int soundIndex = SoundEvent.GetEventIdFromString("YOUR_SOUND_ID");
```


### Several ways to play a sound

Worked ok:

``` cs
mission.MakeSound(soundIndex, Vec3.Zero, false, true, -1, -1);
```

Did not work:

``` cs
SoundEvent.PlaySound2D(soundIndex);
```

Not tested:

``` cs
SoundEvent eventRef = SoundEvent.CreateEvent(soundIndex, mission.Scene);
eventRef.SetPosition(_mission.MainAgent.Position);
eventRef.Play();
```

## Sound in menus

```cs
args.MenuContext.SetPanelSound("event:/ui/panels/settlement_village");
args.MenuContext.SetAmbientSound("event:/map/ambient/node/settlements/2d/village");
```

## Sound Events

Possible to play sound (events) with InformationManager.AddQuickInformation(text, 0, null, <span style="color:green">string SoundEventPath</style>)

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



## Delayed stop to prevent ambient sounds from looping

``` cs
private async void DelayedStop(SoundEvent eventRef)
{
  await Task.Delay(soundDuration);
  eventRef.Stop();
}
```


??? info "Stop looping example"

    ``` cs
    SoundEvent eventRef = SoundEvent.CreateEvent(SoundEvent.GetEventIdFromString(SoundEffectOnUse), Scene);
    eventRef.SetPosition(GameEntity.GetGlobalFrame().origin);
    eventRef.Play();
    DelayedStop(eventRef); // used to prevent ambient sounds from looping
    ```

## NAudio

!!! quote
    Artem: For everyone that tries to add sounds to the game and is annoyed by the games restrictions, like sounds being too short, too quiet etc.

[https://github.com/naudio/NAudio](https://github.com/naudio/NAudio){target=_blank}

``` cs
using NAudio.Wave;

private AudioFileReader audioFile;

if (this.outputDevice == null)
{
    this.outputDevice = new WaveOutEvent();
}

if (this.audioFile == null)
{
    string fullName = Directory.GetParent(Directory.GetParent(Directory.GetParent(Assembly.GetExecutingAssembly().Location).FullName).FullName).FullName;
    this.audioLocation = System.IO.Path.Combine(fullName, "ModuleSounds\\");
    this.audioFile = new AudioFileReader(this.audioLocation + "encounter_river_pirates_9.wav");
    this.audioFile.Volume = NativeOptions.GetConfig(NativeOptions.NativeOptionsType.SoundVolume);
    this.outputDevice.Init(this.audioFile);
}

this.audioFile.Seek(0L, SeekOrigin.Begin);
this.outputDevice.Play();


// stop the sound
if (outputDevice != null)
{
    outputDevice.Stop();
    outputDevice.Dispose();
}

```

Example: Artem's Assassination Mod
