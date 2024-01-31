# Custom Audio with LipSync

<center>
    <iframe width="640" height="360" src="https://www.youtube.com/embed/_9AY7dHWI9o" frameborder="0" allowfullscreen></iframe>
</center>


In this guide I will show how to instruct the character in the game to use our audio and text with lipsync data as in the example above.

I use [ChatGPT](https://chat.openai.com){target=_blank} to generate texts and [Synthesia](https://app.synthesia.io/){target=_blank} to convert text to audio. There are other tools to accomplish the same task. Choose what you like.


Save your generated audio to the OGG file. I use [Audacity](https://www.audacityteam.org/){target=_blank} for this task.


??? "Audacity usage"
    ![](/pics/ajka2ev.png)

    ![](/pics/bUp5TiZ.png)


## File names


Based on the file name game engine selects which file to use. File names should follow the rules. 

File name format: CULTURE_GENDER_VOICETYPE_ID.ogg

??? "VOICETYPE (TW calls them personas) are described in Sandbox\ModuleData\voice_strings.xml. There are 4 of them:"
    curt<br>
    earnest<br>
    ironic<br>
    softspoken


Examples (more in \SandBox\ModuleData\Languages\VoicedLines\EN\PC):

    sturgian_female_curt_150.ogg
    battanian_male_earnest_018.ogg
    aserai_male_ironic_003.xml
    khuzait_softspoken_Q3kWez4P.ogg


??? "VOICETYPE is assigned in XML, like this:"

    ``` xml
    <NPCCharacter id="aserai_troop_civilian_template_t3"
        name="{=!}aserai_troop_civilian_template_t3"
        age="48"
        voice="curt"
    ```

It is important to know the VOICETYPE of the character you want to add custom audio to. If I have male sturgian with VOICETYPE 'earnest', my audio file names will look like this:

    sturgian_male_earnest_ID.ogg

ID can be numeric or made from random letters. TW uses both, check \SandBox\ModuleData\Languages\VoicedLines\EN\PC. As the maximum ID from TW is 020, I used IDs starting from 100 to be safe. Maybe it's a good idea to make ID unique per-mod, so different mods would not conflict.


## LipSync XML

When you have audio file(s) you need, use [Rhubarb Lip Sync](https://github.com/DanielSWolf/rhubarb-lip-sync){target=_blank} to generate XML with LipSync data from your OGG file(s).

??? "I use .bat script to generate LipSync XML from all OGG files at once in D:"

    ```
    @echo off
    setlocal enabledelayedexpansion
    for %%f in (D:\*.ogg) do (
        set "filename=%%~nf"
        echo !filename!
        rhubarb.exe -f xml -o D:\!filename!.xml D:\!filename!.ogg
    )
    endlocal
    ```

![](/pics/ztfpJ1n.png)

XML files should be generated for each OGG file with the same file name. Like this:

    sturgian_male_earnest_101.ogg
    sturgian_male_earnest_101.xml

??? "XML looks like this:"

    ``` xml
    <?xml version="1.0" encoding="utf-8"?>
    <rhubarbResult>
      <metadata>
        <soundFile>D:\sturgian_male_earnest_101.ogg</soundFile>
        <duration>2.05</duration>
      </metadata>
      <mouthCues>
        <mouthCue start="0.00" end="0.15">X</mouthCue>
        <mouthCue start="0.15" end="0.33">F</mouthCue>
        <mouthCue start="0.33" end="0.40">B</mouthCue>
        <mouthCue start="0.40" end="0.54">G</mouthCue>
        <mouthCue start="0.54" end="0.68">C</mouthCue>
        <mouthCue start="0.68" end="0.75">B</mouthCue>
        <mouthCue start="0.75" end="0.89">E</mouthCue>
        <mouthCue start="0.89" end="1.50">X</mouthCue>
        <mouthCue start="1.50" end="1.67">B</mouthCue>
        <mouthCue start="1.67" end="1.81">F</mouthCue>
        <mouthCue start="1.81" end="1.95">B</mouthCue>
        <mouthCue start="1.95" end="2.05">X</mouthCue>
      </mouthCues>
    </rhubarbResult>

    Ignore: <soundFile>D:\sturgian_male_earnest_101.ogg</soundFile>, correct audio file will be used anyway.

    ```

## Folder structure

    \ModuleData\Languages\VoicedLines\EN\PC

    \ModuleData
        \Languages
            language_data.xml
            \VoicedLines
                voiced_lines_en.xml
                \EN
                    \PC
                    XML and OGG files here




??? "\ModuleData\Languages\language_data.xml:"

    ``` xml
    <?xml version="1.0" encoding="utf-8"?>
    <LanguageData id="English">
      <VoiceFile xml_path="VoicedLines/voiced_lines_en.xml" />
    </LanguageData>
    ```

??? "\ModuleData\Languages\VoicedLines\voiced_lines_en.xml:"

    ``` xml
    <?xml version="1.0" encoding="utf-8"?>
    <Base xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" type="string">
        <VoiceOvers>
            <VoiceOver id="AUDIO_ID">
              <Voice path="VoicedLines/EN/$PLATFORM/sturgian_male_earnest_101" />
            </VoiceOver>
        </VoiceOvers>
    </Base>

    ```


Copy your XML/OGG file(s) to \ModuleData\Languages\VoicedLines\EN\PC and add information about it/them into \ModuleData\Languages\VoicedLines\voiced_lines_en.xml



## Playing audio in dialogs


In AddDialogLine conditionDelegate add line:

    MBTextManager.SetTextVariable("VOICED_LINE", "{=AUDIO_ID}" + textLine, false);

AUDIO_ID is from voiced_lines_en.xml <VoiceOver id="AUDIO_ID">. Make your own.

textLine is some text.

??? failure "If you leave textLine empty, you will get such error:"
    ![](/pics/zvLoWia.png)


Then in AddDialogLine use VOICED_LINE like this:

``` cs
starter.AddDialogLine("bookvendor_talk", "bookvendor_yes", "bookvendor_yes_resp", "{=!}{VOICED_LINE}", () =>
{
    string textLine = "Pleasure doing business with you. [ib:hip][rb:positive][rf:happy]";
    MBTextManager.SetTextVariable("VOICED_LINE", "{=AUDIO_ID}" + textLine, false);
    return true;
}, null, 110, null);
```

If everything is set correctly, your character will start moving his mouth based on the played audio.

<br><br><br>
