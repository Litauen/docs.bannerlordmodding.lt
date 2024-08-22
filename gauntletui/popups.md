# Popups / Messages

## Tutorials

* [Custom Popup with Image](/guides/custom_popup)

## DisplayMessage

``` cs
InformationManager.DisplayMessage(new InformationMessage("message", TaleWorlds.Library.Color.ConvertStringToColor("#FF0042FF")));
```

![](/pics/1BK27yg.png)

## ShowInquiry

``` cs
InformationManager.ShowInquiry(new InquiryData("Title", "Text", true, true, "AffirmativeText", "NegativeText", null, null, "event:/ui/notification/peace", 0f, null), true, false);
```

![](/pics/y2cp4rK.png)


## ShowTextInquiry

``` cs
TextInquiryData(
    string titleText,
    string text,
    bool isAffirmativeOptionShown,
    bool isNegativeOptionShown,
    string affirmativeText,
    string negativeText,
    Action<string> affirmativeAction,
    Action negativeAction,
    bool shouldInputBeObfuscated = false,
    Func<string, Tuple<bool, string>> textCondition = null,
    string soundEventPath = "",
    string defaultInputText = "")

ShowTextInquiry(
    TextInquiryData textData,
    bool pauseGameActiveState = false,
    bool prioritize = false)

InformationManager.ShowTextInquiry(new TextInquiryData("Title", "Text", true, true, "AffirmativeText", "NegativeText", null, null, false, null, "", "defaultInputText"), false, false);
```

![](/pics/wgSyy4u.png)


## ShowMultiSelectionInquiry

``` cs
MultiSelectionInquiryData(
  string titleText,
  string descriptionText,
  List<InquiryElement> inquiryElements,
  bool isExitShown,
  int minSelectableOptionCount,
  int maxSelectableOptionCount,
  string affirmativeText,
  string negativeText,
  Action<List<InquiryElement>> affirmativeAction,
  Action<List<InquiryElement>> negativeAction,
  string soundEventPath = "")

MBInformationManager.ShowMultiSelectionInquiry(MultiSelectionInquiryData data);
```

![](/pics/zxK9WCD.png)


Close with: InformationManager.HideInquiry();

??? example "Example with companions:"

    ![](/pics/2408211012.png)

    ```cs
    List<InquiryElement> list = new();

    for (int i = 0; i < Hero.MainHero.PartyBelongedTo.MemberRoster.Count; i++)
    {
        CharacterObject characterAtIndex = Hero.MainHero.PartyBelongedTo.MemberRoster.GetCharacterAtIndex(i);
        if (characterAtIndex.HeroObject != null && characterAtIndex.HeroObject != Hero.MainHero)
        {
            Hero hero = characterAtIndex.HeroObject;

            bool activeItem = true;
            TextObject hint = new("Ready to be selected!");

            if (hero.IsWounded)
            {
                activeItem = false;
                hint = new TextObject("Wounded, can't be selected...", null);
            }

            list.Add(new InquiryElement(hero, hero.Name.ToString(), new ImageIdentifier(CharacterCode.CreateFrom(characterAtIndex)), activeItem, hint.ToString()));
        }
    }

    MultiSelectionInquiryData data = new(new TextObject("Select the companion:").ToString(), "",
        list, true, 0, 1, new TextObject("Select").ToString(), new TextObject("Leave").ToString(), (List<InquiryElement> list) =>
    {
        foreach (InquiryElement inquiryElement in list)
        {
            if (inquiryElement != null && inquiryElement.Identifier != null)
            {
                Hero? hero = inquiryElement.Identifier as Hero;
                if (hero != null)
                {
                    InformationManager.DisplayMessage(new InformationMessage($"Companion selected: {hero.Name}", TaleWorlds.Library.Color.ConvertStringToColor("#FF0042FF")));
                    //GameMenu.SwitchToMenu("town");
                }
            }
        }
    }, (List<InquiryElement> list) => { }, "");

    MBInformationManager.ShowMultiSelectionInquiry(data);
    ```

## AddQuickInformation

![](/pics/dRrDaYF.png)

``` cs
MBInformationManager.AddQuickInformation(TextObject content, int priority, CharacterObject announcer, string sounEventPath);
```

Priority determines how long the message stays, approximate values:

- 0 - 2.5s
- 1000 - 3.5s
- 5000 - 7.5s
- 10000 - 13s
