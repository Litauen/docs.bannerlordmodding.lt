# CampaignEvents

## Start events

### OnNewGameCreatedEvent

Only when new game is started

``` cs
CampaignEvents.OnNewGameCreatedEvent.AddNonSerializedListener(this, new Action<CampaignGameStarter>(this.OnNewGameCreated));
```

### OnSessionLaunchedEvent

On new game + on old game loaded

``` cs
CampaignEvents.OnSessionLaunchedEvent.AddNonSerializedListener(this, new Action<CampaignGameStarter>(this.OnSessionLaunched));
```


## Periodic events


### HourlyTickEvent

Every hour

```cs
CampaignEvents.HourlyTickEvent.AddNonSerializedListener(this, HourlyTickEvent);
```

### DailyTickEvent

Every day

```cs
CampaignEvents.DailyTickEvent.AddNonSerializedListener(this, DailyTickEvent);
```


### WeeklyTickEvent

Every week (7 days)

```cs
CampaignEvents.WeeklyTickEvent.AddNonSerializedListener(this, WeeklyTickEvent);
```


## Template

```cs
using System;
using TaleWorlds.CampaignSystem;


namespace Your_Workspace
{
    internal class TemplateBehavior : CampaignBehaviorBase
    {
        public static TemplateBehavior? Instance { get; set; }

        readonly bool _debug = false;


        public TemplateBehavior()
        {
            Instance = this;
        }



        public override void SyncData(IDataStore dataStore) { }

        public override void RegisterEvents()
        {
            CampaignEvents.OnSessionLaunchedEvent.AddNonSerializedListener(this, new Action<CampaignGameStarter>(this.OnSessionLaunched));
            CampaignEvents.OnNewGameCreatedEvent.AddNonSerializedListener(this, new Action<CampaignGameStarter>(this.OnNewGameCreated));

            CampaignEvents.HourlyTickEvent.AddNonSerializedListener(this, HourlyTickEvent);
            CampaignEvents.DailyTickEvent.AddNonSerializedListener(this, DailyTickEvent);
            CampaignEvents.WeeklyTickEvent.AddNonSerializedListener(this, WeeklyTickEvent);
        }

        private void OnNewGameCreated(CampaignGameStarter starter)
        {
        }


        private void OnSessionLaunched(CampaignGameStarter starter)
        {
        }

        private void HourlyTickEvent()
        {
        }

        private void DailyTickEvent()
        {
        }

        private void WeeklyTickEvent()
        {
        }

    }
}

```