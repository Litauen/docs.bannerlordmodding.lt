# Time

``` cs
int hour = (int)CampaignTime.Now.CurrentHourInDay;
```

Now, Never, IsFuture, IsPast, IsNow, IsDayTime, IsNightTime


## Get details

GetHourOfDay, GetDayOfWeek, GetDayOfSeason, GetDayOfYear, GetSeasonOfYear, GetYear



## Time diff from/to in normal time units

CampaignTime.ElapsedDaysUntilNow (Milliseconds, Seconds, Hours, Days, Weeks, Seasons, Years)

RemainingMillisecondsFromNow ...



## Time diff from/to in CampaignTime

MillisecondsFromNow, SecondsFromNow, MinutesFromNow, HoursFromNow, DaysFromNow, WeeksFromNow, YearsFromNow



## Conversion to normal Time units

ToMilliseconds, ToSeconds. ToMinutes, ToHours, ToDays, ToWeeks, ToSeasons, ToYears



## Conversion to CampaignTime

public static CampaignTime Milliseconds(long valueInMilliseconds), Seconds, Minutes, Hours, Days, Weeks, Seasons, Years