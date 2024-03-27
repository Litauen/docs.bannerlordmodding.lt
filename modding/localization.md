# Localization / Translations

## Guides/Tutorials/Tools

* [Community doc - Localization](https://docs.bannerlordmodding.com/_csharp-api/localization/){target=_blank}
* [Translation Support - Lesser Scholar video](https://www.youtube.com/watch?v=46qr2yCo4lg&list=PLzebdAxJeltRwfJ8jzsNolgHkRvLjoCRC&index=19){target=_blank}
* [Localization Parser](https://forums.taleworlds.com/index.php?threads/tool-localization-parser.444549/) from the game's libraries or from a module's libraries to a .csv file


## Folder structure

    \YourModName\ModuleData\Languages\
        std_module_strings_xml.xml
        LANGUAGE_FOLDER
            language_data.xml
            std_module_strings_xml.xml

Check Native: \Modules\Native\ModuleData\Languages


??? "Where LANGUAGE_FOLDER - short code for the language:"
    BR<br>
    CNs<br>
    CNt<br>
    DE<br>
    FR<br>
    IT<br>
    JP<br>
    KO<br>
    PL<br>
    RU<br>
    SP<br>
    TR<br>


??? "language_data.xml example:"

    ``` xml
    <?xml version="1.0" encoding="utf-8"?>
    <LanguageData id="Français">
      <LanguageFile xml_path="FR/std_module_strings_xml.xml" />
    </LanguageData>
    ```

??? "std_module_strings_xml.xml example:"

    ``` xml
    <?xml version="1.0" encoding="utf-8"?>
    <base xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:xsd="http://www.w3.org/2001/XMLSchema" type="string">
        <tags>
            <tag language="Português (BR)" />
        </tags>
        <strings>
            <string id="LT001" text="LT_Educação carregada"/>
            <string id="LT002" text="LT_Education: Ocorreu um erro ao tentar carregar o mod em seu jogo atual."/>
            <string id="LT003" text="Você pode ler!"/>
        </strings>
    </base>
    ```

## HTML Tags

??? danger "< > symbols will stop localization file to be loaded"

    ```
    Use &lt; &gt; instead in the localization files.
    ```
    These 'codes' does not work in the game directly, use < > for the original English text.


    Other symbols used in the localization files:

    ```
    &amp; &quot;
    ```

    [List of character entity references in HTML](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references#Character_entity_references_in_HTML){target=_blank}

Translations support HTML tags:

    &lt;b&gt;Fire Mangonel&lt;/b&gt;

New line:

    {newline}


## Prefabs

[NEED CONFIRMATION] It seems that localization not possible directly in the GUI/Prefabs/.xml files. All text strings should come translated from VM.
