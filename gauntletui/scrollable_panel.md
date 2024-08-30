# Scrollable Panel

Code by [carbon](https://discord.com/channels/411286129317249035/677511186295685150/1278698466733981716)

![](/pics/2408291641.png)



!!! quote "carbon"
    ... But if you or anyone else wants to see a dynamic scrollable panel, i have an example you can use [here](https://gist.github.com/carbon198/7d8c7e15c787177e188a0bb1fbfcf228)
    <br>
    You'd just replace all that stuff i have there with one TextWidget. And you can add/remove from the datasource (PartyList in this case), and trigger OnPropertyChanged("PartyList"); to make the UI refresh from the datasource
    <br>
    TW prefers putting it in the Setter of the data source. But since the data source is a list I prefer to trigger it manually


``` xml
<ScrollablePanel WidthSizePolicy="CoverChildren" HeightSizePolicy="StretchToParent" MarginLeft="3" MarginBottom="3" AutoHideScrollBars="true" ClipRect="PartyListClipRect" InnerPanel="PartyListClipRect\PartyList" VerticalScrollbar="..\PartyListScrollbar\Scrollbar">
  <Children>
    <NavigationScopeTargeter ScopeID="PartyAIControlsPanelItemsContext" ScopeParent="..\PartyListClipRect" ScopeMovements="Vertical" />
    <Widget Id="PartyListClipRect" WidthSizePolicy="CoverChildren" HeightSizePolicy="StretchToParent" ClipContents="true">
      <Children>
        <NavigatableListPanel Id="PartyList" DataSource="{PartyList}" WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" StackLayout.LayoutMethod="VerticalBottomToTop">
          <ItemTemplate>
              <ListPanel WidthSizePolicy="CoverChildren" HeightSizePolicy="Fixed" SuggestedHeight="!PartyRow.Height" MarginTop="!PartyRow.Spacing" UpdateChildrenStates="true" StackLayout.LayoutMethod="HorizontalLeftToRight">
                <Children>
                    <!-- Clan Banner -->
                    <Widget DataSource="{ClanHint}" DoNotPassEventsToChildren="true" WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" MarginRight="5" Command.HoverBegin="ExecuteBeginHint" Command.HoverEnd="ExecuteEndHint">
                      <Children>
                        <MaskedTextureWidget DataSource="{..\ClanVisual}" WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="29" SuggestedHeight="37" HorizontalAlignment="Right" VerticalAlignment="Top" Brush="Kingdom.OtherWars.Faction.Small.Banner" AdditionalArgs="@AdditionalArgs" ImageId="@Id" ImageTypeCode="@ImageTypeCode" IsDisabled="true"  />
                      </Children>
                    </Widget>

                    <!-- Portrait -->
                    <ButtonWidget DoNotPassEventsToChildren="true" WidthSizePolicy="CoverChildren" HeightSizePolicy="StretchToParent" Command.AlternateClick="OpenEncyclopediaLink">
                        <Children>
                            <ImageIdentifierWidget DataSource="{Visual}" WidthSizePolicy="Fixed" HeightSizePolicy="StretchToParent" SuggestedWidth="46" AdditionalArgs="@AdditionalArgs" ImageId="@Id" ImageTypeCode="@ImageTypeCode" />
                        </Children>
                    </ButtonWidget>

                    <!-- Hero Name -->
                    <TextWidget WidthSizePolicy="Fixed" HeightSizePolicy="StretchToParent" SuggestedWidth="300" HorizontalAlignment="Left" VerticalAlignment="Bottom" Brush="PartyAI.Menu.PartyRowText" Text="@LeaderName"/>

                    <!--In Army Icon-->
                    <Widget HorizontalAlignment="Left" WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="34" SuggestedHeight="!PartyRow.Height" VerticalAlignment="Center" MarginRight="10" >
                      <Children>
                        <BrushWidget  WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" VerticalAlignment="Center" Brush="SPClan.Members.InArmyIcon" IsVisible="@IsInArmy" >
                          <Children>
                            <HintWidget DataSource="{InArmyHint}" WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" Command.HoverBegin="ExecuteBeginHint" Command.HoverEnd="ExecuteEndHint" IsDisabled="true" />
                          </Children>
                        </BrushWidget>
                        <Widget WidthSizePolicy="StretchToParent" HeightSizePolicy="Fixed" SuggestedHeight="21" HorizontalAlignment="Center" VerticalAlignment="Center" Sprite="Clan\party_leader_crown" Color="#be945bff" IsVisible="@IsArmyLeader">
                          <Children>
                            <HintWidget DataSource="{ArmyLeaderHint}" WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" Command.HoverBegin="ExecuteBeginHint" Command.HoverEnd="ExecuteEndHint" IsDisabled="true" />
                          </Children>
                        </Widget>
                      </Children>
                    </Widget>

                    <!-- Party Size -->
                    <TextWidget WidthSizePolicy="Fixed" HeightSizePolicy="StretchToParent" SuggestedWidth="110" HorizontalAlignment="Left" VerticalAlignment="Bottom" MarginRight="10" Brush="PartyAI.Menu.PartyRowText" Text="@PartySize" Command.HoverBegin="PartySizeBeginHint" Command.HoverEnd="PartySizeEndHint" />

                    <!-- Party Composition Targets -->
                    <PartyAICompositionDisplay Parameter.DataSource="{PartyComposition}" HorizontalAlignment="Left" HeightSizePolicy="Fixed" SuggestedHeight="!PartyRow.Height" WidthSizePolicy="Fixed" SuggestedWidth="375" />

                    <!-- Edit Party Composition -->
                    <ButtonWidget DoNotPassEventsToChildren="true" WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="!PartyRow.Height" SuggestedHeight="!PartyRow.Height" MarginLeft="-55" UpdateChildrenStates="true" Command.Click='EditPartyComposition'>
                      <Children>
                        <HintWidget DataSource="{EditHint}" WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" Command.HoverBegin="ExecuteBeginHint" Command.HoverEnd="ExecuteEndHint" />
                        <BrushWidget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="!PartyRow.Height" SuggestedHeight="!PartyRow.Height" HorizontalAlignment="Right" VerticalAlignment="Center" Brush="SPClan.NameChange.Icon" />
                      </Children>
                    </ButtonWidget>

                    <!-- Template Name -->
                    <TextWidget WidthSizePolicy="Fixed" HeightSizePolicy="StretchToParent" MarginLeft="20" MarginRight="20" SuggestedWidth="150" HorizontalAlignment="Left" VerticalAlignment="Bottom" Brush="PartyAI.Menu.PartyRowText" Brush.TextHorizontalAlignment="Right" Text="@TemplateName"/>

                    <!-- Edit Party Template -->
                    <ButtonWidget DoNotPassEventsToChildren="true" WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="!PartyRow.Height" SuggestedHeight="!PartyRow.Height" MarginLeft="-10" UpdateChildrenStates="true" Command.Click='EditPartyTemplate'>
                      <Children>
                        <HintWidget DataSource="{ChangeHint}" WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" Command.HoverBegin="ExecuteBeginHint" Command.HoverEnd="ExecuteEndHint" />
                        <BrushWidget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="!PartyRow.Height" SuggestedHeight="!PartyRow.Height" HorizontalAlignment="Right" VerticalAlignment="Center" Brush="SPClan.NameChange.Icon" />
                      </Children>
                    </ButtonWidget>

                    <!-- Active Order -->
                    <TextWidget WidthSizePolicy="Fixed" HeightSizePolicy="StretchToParent" MarginLeft="0" MarginRight="20" SuggestedWidth="200" HorizontalAlignment="Left" VerticalAlignment="Bottom" Brush="PartyAI.Menu.PartyRowText" Brush.TextHorizontalAlignment="Right" Text="@ActiveOrder"/>

                    <!-- Edit Active Order -->
                    <Widget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="!PartyRow.Height" SuggestedHeight="!PartyRow.Height" MarginLeft="-10" >
                        <Children>
                            <ButtonWidget DoNotPassEventsToChildren="true" WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" UpdateChildrenStates="true" Command.Click='ChangeActiveOrder' IsVisible="@CanShowLocationOfHero">
                              <Children>
                                <HintWidget DataSource="{ChangeHint}" WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" Command.HoverBegin="ExecuteBeginHint" Command.HoverEnd="ExecuteEndHint" />
                                <BrushWidget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="!PartyRow.Height" SuggestedHeight="!PartyRow.Height" HorizontalAlignment="Right" VerticalAlignment="Center" Brush="SPClan.NameChange.Icon" />
                              </Children>
                            </ButtonWidget>
                        </Children>
                    </Widget>

                    <!-- Edit Party Options -->
                    <ButtonWidget DoNotPassEventsToChildren='true' WidthSizePolicy='CoverChildren' HeightSizePolicy='Fixed' SuggestedHeight='!PartyRow.Height' Brush='ButtonBrush2' VerticalAlignment="Bottom" HorizontalAlignment='Left' UpdateChildrenStates='true' MarginLeft="40" MarginRight="25" Command.Click='EditPartyOptions'>
                        <Children>
                            <TextWidget WidthSizePolicy='CoverChildren' HeightSizePolicy='StretchToParent' Brush='Kingdom.GeneralButtons.Text' Text='@OptionsText' />
                        </Children>
                    </ButtonWidget>

                    <!--Show On Map Button-->
                    <ButtonWidget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="33" SuggestedHeight="19" HorizontalAlignment="Left" VerticalAlignment="Center" Brush="ShowOnMap.Button" Command.Click="ShowHeroOnMap" IsVisible="@CanShowLocationOfHero" DoNotPassEventsToChildren="true" UpdateChildrenStates="true" GamepadNavigationIndex="0">
                      <Children>
                        <HintWidget DataSource="{ShowOnMapHint}" WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" Command.HoverBegin="ExecuteBeginHint" Command.HoverEnd="ExecuteEndHint" />
                      </Children>
                    </ButtonWidget>
                </Children>
              </ListPanel>
          </ItemTemplate>
        </NavigatableListPanel>
      </Children>
    </Widget>

  </Children>
</ScrollablePanel>

<Standard.VerticalScrollbar Id="PartyListScrollbar" HeightSizePolicy="StretchToParent" HorizontalAlignment="Right" MarginRight="2" MarginLeft="2" MarginBottom="3" />
```