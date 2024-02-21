# Dynamic Weather Effects

<center>
![](/pics/ee_snow_rain.gif)
</center>


Set in the Terrain Properties (Alt+7):

![](/pics/2402201751.png)


If you need - download vanilla main_map_snow_flowmap.png [here](/pics/main_map_snow_flowmap.png)

This image encodes different information in Red, Green and Blue channels:

- <span style="color:red">**Red**</span> is for snow
- <span style="color:green">**Green**</span> is for clouds and precipitation
- <span style="color:blue">**Blue**</span> is where you don't want snow

<center>
![](/pics/weather_channels.gif)
</center>

## <span style="color:red">**Red Channel**</span>

This channel encodes information where and when snow will appear. White color intensity determines when snow will be shown.

For example: intensity 70-75 is for a snow only in the winter. 255 - for eternal snow.

![](/pics/2402201805.png)

For the best-fastest result I used Photoshop's Clone Stamp Tool to create my snow map from the vanilla one:

![](/pics/2402201809.png)



## <span style="color:green">**Green Channel**</span>

This channel is for for clouds and precipitation.

I don't know how to decode the vanilla bubbles, so again - copy/pasted to cover the whole area:

![](/pics/2402201813.png)


## <span style="color:blue">**Blue Channel**</span>


This channel is where you don't want snow. It's a simple mask, where white - zones where no snow will be present.

![](/pics/2402202201.png)

Usually used to cover the sea beds and lakes.

I used different levels of white (100-150-200) for different river coverage in winter:

![](/pics/2402201822.png)
