# Read data from XML

```cs
public class Religion : MBObjectBase
{
    public TextObject Name { get; set; }
    public TextObject Description { get; private set; }
    public float Fervor { get; private set; }
    public List<CultureObject> Cultures { get; private set; } = new List<CultureObject>();
    public List<string> InitialHeroes { get; private set; } = new List<string>();

    public override void Deserialize(MBObjectManager objectManager, XmlNode node)
    {
        base.Deserialize(objectManager, node);

        this.Name = new TextObject(node.Attributes.GetNamedItem("Name").Value, null);

        float fervor = 0;
        if (float.TryParse(node.Attributes.GetNamedItem("Fervor").Value, out fervor)) this.Fervor = fervor;

        // read long descriptiont from the separate ee_strings.xml file
        this.Description = GameTexts.FindText("ee_religion_description", base.StringId);

        if (node.HasChildNodes)
        {
            foreach (object obj in node.ChildNodes)
            {
                XmlNode xmlNode = (XmlNode)obj;
                if (xmlNode.Name == "Cultures")
                {
                    foreach (object obj2 in xmlNode.ChildNodes)
                    {
                        XmlNode xmlNode2 = (XmlNode)obj2;
                        if (xmlNode2.Name == "Culture")
                        {
                            CultureObject cultureObject = MBObjectManager.Instance.ReadObjectReferenceFromXml<CultureObject>("id", xmlNode2);
                            if (cultureObject != null)
                            {
                                this.Cultures.Add(cultureObject);
                            }
                        }
                    }
                }

                if (xmlNode.Name == "Heroes")
                {
                    foreach (object obj3 in xmlNode.ChildNodes)
                    {
                        XmlNode xmlNode3 = (XmlNode)obj3;
                        if (xmlNode3.Name == "Hero")
                        {
                            string value = xmlNode3.Attributes.GetNamedItem("id").Value;
                            if (!string.IsNullOrWhiteSpace(value) && !this.InitialHeroes.Contains(value))
                            {
                                this.InitialHeroes.Add(value);
                            }
                        }
                    }
                }
            }
        }
    }
}
```

??? abstract "Data XML file"
    ```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <Religions>
        <Religion id="baltic_paganism" Name="Baltic Paganism" Fervor="40">
            <Cultures>
                <Culture id="Culture.baltic" />
                <Culture id="Culture.latvian" />
                <Culture id="Culture.estonian" />
            </Cultures>
            <Heroes></Heroes>
        </Religion>

        <Religion id="catholicism" Name="Catholicism" Fervor="70">
            <Cultures>
                <Culture id="Culture.crusader" />
            </Cultures>
            <Heroes>
                <Hero id="lord_lit_1_2" />
            </Heroes>
        </Religion>

        <Religion id="orthodoxy" Name="Orthodoxy" Fervor="60">
            <Cultures>
                <Culture id="Culture.rus" />
            </Cultures>
            <Heroes>
                <Hero id="lord_lit_1_5" />
            </Heroes>
        </Religion>
    </Religions>
    ```

??? abstract "Strings XML file"
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <strings>
        <string id="ee_religion_description.baltic_paganism" text= "{=!ee_religion_description.baltic_paganism}Baltic Paganism is the pre-Christian religion ..."/>
        <string id="ee_religion_description.catholicism" text= "{=!ee_religion_description.catholicism}Catholicism is a major branch of Christianity that ..." />
        <string id="ee_religion_description.orthodoxy" text= "{=!ee_religion_description.orthodoxy}Orthodoxy is a branch of Christianity that ..." />
    </strings>
    ```

??? abstract "Include those files in Submodule.xml"
    ```xml
    <XmlNode>
        <XmlName id="Religions" path="ee_religions" />
        <IncludedGameTypes>
            <GameType value="Campaign" />
            <GameType value="CampaignStoryMode" />
            <GameType value = "CustomGame"/>
            <GameType value = "EditorGame"/>
        </IncludedGameTypes>
    </XmlNode>

    <XmlNode>
        <XmlName id="GameText" path="ee_strings"/>
        <IncludedGameTypes>
            <GameType value = "Campaign"/>
            <GameType value = "CampaignStoryMode"/>
            <GameType value = "CustomGame"/>
            <GameType value = "EditorGame"/>
        </IncludedGameTypes>
    </XmlNode>
    ```



Load data from XML in SubModule.cs:

``` cs
public override void BeginGameStart(Game game)
{
    if (game.GameType is Campaign)
    {
        game.ObjectManager.RegisterType<Religion>("Religion", "Religions", 100U, true, false);
        MBObjectManager.Instance.LoadXML("Religions", false);
    }
}
```


!!! failure "RGL log will complain that xsd file of YOURFILE.xml could not be found!"
    Tried to include XSD schema directly into the XML - did not work.

    Tried to include the external file - did not work:
    ```xml
    <?xml version="1.0" encoding="utf-8"?>
        <Religions xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:noNamespaceSchemaLocation="Religions.xsd">
    ```

    \Mount & Blade II Bannerlord\XmlSchemas folder is present, but no idea how to tell the game to look for my XSD schema file in the Mod's folder.
