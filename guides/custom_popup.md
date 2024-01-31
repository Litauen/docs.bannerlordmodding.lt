# Custom Popup with Image

![](/pics/tLujFvY.png)



??? "YOURViewModel Class"

    ``` cs
    public class YOURPopupVM : ViewModel
    {
        private string _title;
        private string _smallText;
        private string _spriteName;

        public YOURPopupVM(string title, string smallText, string spriteName)
        {

            this.Title = title;
            this.SmallText = smallText;
            this.SpriteName = spriteName;
        }

        public void Close()
        {
            YOURBehaviour.DeletePopupVMLayer();
        }

        public void Refresh()
        {
            this.Title = _title;
            this.SmallText = _smallText;
            this.SpriteName = _spriteName;
        }


        public string Title
        {
            get
            {
                return this._title;
            }
            set
            {
                this._title = value;
                base.OnPropertyChangedWithValue(value, "PopupTitle");
            }
        }

        public string SmallText
        {
            get
            {
                return this._smallText;
            }
            set
            {
                this._smallText = value;
                base.OnPropertyChangedWithValue(value, "PopupSmallText");
            }
        }

        public string SpriteName
        {
            get
            {
                return this._spriteName;
            }
            set
            {
                this._spriteName = value;
                base.OnPropertyChangedWithValue(value, "SpriteName");
            }
        }

    }
    ```



??? "YOURCampaignBehaviour Class"
    ``` cs
    public class YOURBehaviour : CampaignBehaviorBase
    {
        private static GauntletLayer _gauntletLayer;
        private static GauntletMovie _gauntletMovie;
        private static YOURPopupVM _popupVM;

        public static void CreatePopupVMLayer(string title, string smallText, string spriteName)
        {
            try
            {
                bool flag = YOURBehaviour._gauntletLayer != null;
                if (!flag)
                {
                    YOURBehaviour._gauntletLayer = new GauntletLayer(1000, "GauntletLayer", false);
                    bool flag2 = _popupVM == null;
                    if (flag2)
                    {
                        YOURBehaviour._popupVM = new YOURPopupVM(title, smallText, spriteName);
                    }
                    YOURBehaviour._gauntletMovie = (GauntletMovie)YOURBehaviour._gauntletLayer.LoadMovie("YOURPopupXMLFILENAME_WITHOUT_EXTENSION", YOURBehaviour._popupVM);
                    YOURBehaviour._gauntletLayer.InputRestrictions.SetInputRestrictions(true, InputUsageMask.All);
                    ScreenManager.TopScreen.AddLayer(YOURBehaviour._gauntletLayer);
                    YOURBehaviour._gauntletLayer.IsFocusLayer = true;
                    ScreenManager.TrySetFocus(YOURBehaviour._gauntletLayer);
                    YOURBehaviour._popupVM.Refresh();   // does not work without this for me :(
                }
            }
            catch (Exception ex)
            {
                // do smthg
            }
        }


        public static void DeletePopupVMLayer()
        {
            ScreenBase topScreen = ScreenManager.TopScreen;
            bool flag = YOURBehaviour._gauntletLayer != null;
            if (flag)
            {
                YOURBehaviour._gauntletLayer.InputRestrictions.ResetInputRestrictions();
                YOURBehaviour._gauntletLayer.IsFocusLayer = false;
                bool flag2 = YOURBehaviour._gauntletMovie != null;
                if (flag2)
                {
                    YOURBehaviour._gauntletLayer.ReleaseMovie(YOURBehaviour._gauntletMovie);
                }
                topScreen.RemoveLayer(YOURBehaviour._gauntletLayer);
            }
            YOURBehaviour._gauntletLayer = null;
            YOURBehaviour._gauntletMovie = null;
            YOURBehaviour._popupVM = null;
        }




    ```



??? "Popup XML Prefab (YOURPopupXMLFILENAME.xml) - should be in ../GUI/Prefabs"
    ``` xml
    <Prefab>
    <Constants>
    </Constants>
    <Window>
        <Widget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" Sprite="BlankWhiteSquare_9" Color="#00000000">
            <Children>
                <!--Background-->
                <DimensionSyncWidget Id="SingleQueryPopupParent" WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="830" SuggestedHeight="650" HorizontalAlignment="Center" VerticalAlignment="Center" PositionYOffset="-20" DimensionToSync="Vertical">
                    <Children>
                        <!-- background -->
                        <DimensionSyncWidget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" SuggestedWidth="830" ClipContents="true" DimensionToSync="Vertical">
                            <Children>
                                <Widget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" SuggestedWidth="830" SuggestedHeight="650" HorizontalAlignment="Center" VerticalAlignment="Top" Sprite="StdAssets\Popup\canvas" />

                                <!-- image -->
                                <ImageWidget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" HorizontalAlignment="Center" VerticalAlignment="Center" SuggestedWidth="600" SuggestedHeight="400" PositionYOffset="20" Sprite="@SpriteName"/>

                                <!-- image frame -->
                                <Widget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" HorizontalAlignment="Center" VerticalAlignment="Center" SuggestedWidth="600" SuggestedHeight="400" PositionYOffset="20" Sprite="frame_9" ExtendLeft="18" ExtendTop="18" ExtendRight="18" ExtendBottom="18" IsEnabled="false" />

                                <!-- text with big font -->
                                <!-- <TextWidget WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" HorizontalAlignment="Center" VerticalAlignment="Center" PositionYOffset="-230" Text="@PopupBigText" /> -->

                                <!-- text with several lines -->
                                <!-- <RichTextWidget WidthSizePolicy="StretchToParent" HeightSizePolicy="CoverChildren" HorizontalAlignment="Center" PositionYOffset="60" MarginLeft="50" MarginRight="50" MarginTop="20" MarginBottom="20" Brush="Popup.Description.Text" MaxHeight="480" MinHeight="10" Text="@PopupMultiText" /> -->

                                <!-- text with small font -->
                                <TextWidget WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" HorizontalAlignment="Center" VerticalAlignment="Center" PositionYOffset="-230" Brush="Popup.Description.Text" Text="@PopupSmallText" />

                                <!-- text on image
                                <TextWidget WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" HorizontalAlignment="Center" VerticalAlignment="Center" PositionYOffset="150" Text="@PopupImageText" />-->

                                <!-- gradient from bottom to the top -->
                                <Widget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" Sprite="StdAssets\Popup\canvas_gradient" IsEnabled="false" />

                                <!--Close Button-->
                                <ListPanel WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" HorizontalAlignment="Center" MarginTop="575">
                                    <Children>
                                        <ButtonWidget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedHeight="48" SuggestedWidth="240" HorizontalAlignment="Center" VerticalAlignment="Bottom" Brush="ButtonBrush1" Command.Click="Close" IsEnabled="true">
                                            <Children>
                                                <RichTextWidget DoNotAcceptEvents="true" WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" HorizontalAlignment="Center" VerticalAlignment="Center" Brush="OverlayPopup.ButtonText" Brush.FontSize="20" Text="Continue" />
                                            </Children>
                                        </ButtonWidget>
                                    </Children>
                                </ListPanel>
                            </Children>
                        </DimensionSyncWidget>

                        <!-- frame -->
                        <Widget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" Sprite="frame_9" ExtendLeft="18" ExtendTop="18" ExtendRight="18" ExtendBottom="18" IsEnabled="false" />

                        <!--title -->
                        <Widget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="590" SuggestedHeight="125" HorizontalAlignment="Center" VerticalAlignment="Top" PositionXOffset="6" PositionYOffset="-28" Sprite="StdAssets\tabbar_popup" IsDisabled="true">
                            <Children>
                                <RichTextWidget WidthSizePolicy="CoverChildren" HeightSizePolicy="CoverChildren" HorizontalAlignment="Center" VerticalAlignment="Center" PositionYOffset="-16" Brush="Recruitment.Popup.Title.Text" Brush.FontSize="34" IsDisabled="true" Text="@PopupTitle" />
                            </Children>
                        </Widget>
                    </Children>
                </DimensionSyncWidget>
            </Children>
        </Widget>
    </Window>
    </Prefab>
    ```
Make sure you have your [sprite loaded properly into the game](/gauntletui/sprites). Sprite's size in this example is 600x400.

Create popup with command:

``` cs
YOURBehaviour.CreatePopupVMLayer("Hot Springs", "You stumble upon some beautiful hot springs. After bathing with your soldiers you feel fantastic!", "hot_springs");
```
