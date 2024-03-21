# Custom Progress bar

<center>
![](/pics/horse_bar.gif)
</center>

Made by [Artem](https://discord.com/channels/411286129317249035/411286129317249038/1219584814907260961)

``` cs
public class HuntsMissionScaredCounterVM : ViewModel
{

    public HuntsMissionScaredCounterVM(Mission mission)
    {
        this.RefreshValues();
    }

    public void Tick()
    {
        if (Agent.Main != null && Agent.Main.MountAgent != null)
        {
            int scaredCount = HuntsHorseAgentComponent.Instance.horseScaredCounter;
            ScaredCounter = scaredCount;
            IsVisible = true;
        }
        else
        {
            IsVisible = false;
        }
    }

    public override void RefreshValues()
    {
        base.RefreshValues();
        IsVisible = _isVisible;
    }

    [DataSourceProperty]
    public bool IsVisible
    {
        get
        {
            return this._isVisible;
        }
        set
        {
            if (value != this._isVisible)
            {
                this._isVisible = value;
                base.OnPropertyChangedWithValue(value, "IsVisible");
            }
        }
    }

    [DataSourceProperty]
    public int ScaredCounter
    {
        get
        {
            return this._scaredCounter;
        }
        set
        {
            if (value != this._scaredCounter)
            {
                this._scaredCounter = value;
                base.OnPropertyChangedWithValue(value, "ScaredCounter");
            }
        }
    }
    bool _isVisible = false;
    public int _scaredCounter;
}
```

``` xml
<Prefab>
  <Window>
    <Widget HeightSizePolicy ="CoverChildren" WidthSizePolicy="CoverChildren"  HorizontalAlignment="Center" VerticalAlignment="Bottom" MarginBottom="500" IsVisible="@IsVisible" IsVertical="false" ScaledPositionYOffset="430" ScaledPositionXOffset="760">
      <Children>
        <Widget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="160" SuggestedHeight="20" VerticalAlignment="Center" Filler="Filler" Handle="SliderHandle" Locked="false" IsVertical="false">
          <Children>
            <Widget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="200" SuggestedHeight="20" HorizontalAlignment="Center" VerticalAlignment="Center" Sprite="General\Mission\horse_canvas" IsEnabled="true" />
            <Widget DoNotPassEventsToChildren="true" WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="150" SuggestedHeight="0" VerticalAlignment="Center" HorizontalAlignment="Center" ScaledPositionXOffset="10" Color="#00000000">
                    <Children>
                        <FillBar WidthSizePolicy="StretchToParent" HeightSizePolicy="Fixed" SuggestedHeight="10" VerticalAlignment="Center" PositionYOffset="2" MaxAmount="100" CurrentAmount="@ScaredCounter" InitialAmount="@ScaredCounter" Brush="BoundaryCrossing.FillBar.Fill"  />                 
                        <BrushWidget WidthSizePolicy="StretchToParent" HeightSizePolicy="StretchToParent" />
                    </Children>
                </Widget>
            <Widget WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="200" SuggestedHeight="20" HorizontalAlignment="Center" VerticalAlignment="Center" Sprite="General\Mission\horse_frame" IsEnabled="true" />
            <Widget Id="SliderHandle" DoNotAcceptEvents="true" WidthSizePolicy="Fixed" HeightSizePolicy="Fixed" SuggestedWidth="14" SuggestedHeight="38" HorizontalAlignment="Left" VerticalAlignment="Center" IsVisible="true" />
          </Children>
        </Widget>
    </Children>
    </Widget>
  </Window>
</Prefab>
```

