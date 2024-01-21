# Custom Culture Selection Screen

![](https://imgur.com/c2soKfU.png)


On new game start character culture can be selected if it's marked in the culture XML as is_main_culture="true":

``` xml
<Culture
    id="baltic"
    name="Baltic"
    is_main_culture="true"
```

##Culture's Selection Cards

![](https://imgur.com/dAbzEt7.png)


The culture's name that is visible under the picture is defined in the module_strings.xml :

``` xml
<string id="str_culture_rich_name.baltic" text="Baltic" />
```

![](https://imgur.com/nnjvduV.png)


Image dimensions for the culture's picture: 366 x 668

I leave some transparent space at the bottom to not cover the culture's name.

Also round the corners, so the highlight would not look weird.

![](https://imgur.com/cPK9nuC.png)

These images should be named the same as culture ID:

``` xml
<Culture
    id="baltic"
```

Image name: baltic.png

Sprites should be [imported](/gauntletui/sprites/#import) from: 

    \MODULE_NAME\GUI\SpriteParts\ui_charactercreation\CharacterCreation/Culture\

That means that we are overwriting the native sprite category: ui_charactercreation

and vanilla culture images will be gone. If we want them back - export them with [TpacTool/BannerEdge](/resources/tools/) and reimport them together with your new culture's sprites with the correct names: battania, aserai, etc.

The game uses these sprites for your new cultures automatically - nothing to be done here anymore.


## Culture's Description

![](https://imgur.com/SYqApNY.png)


The moving images are constructed out of 4 sprites. Each of them is stacked on top of another as visible in this example:

![](https://imgur.com/2zkCAS4.png)

4 at the bottom, does not move. Then sprite #3 - moves very slightly, #2 - moves more and #1 on the top, moves the most.

These sprites move horizontally at different speeds trying to create an illusion.

We will need 4 separate images (with some transparency to not cover each other completely) with the names: CULTURE_1, CULTURE_2, CULTURE_3, CULTURE_4, where CULTURE is your culture's ID, for example: baltic_1, baltic_2, baltic_3, baltic_4

Dimensions for a picture: 952 x 876

Leave some transparent space at the top/bottom for a better look. The full image start at the top of the screen and goes behind the text - not very nice.


These sprites should be imported from the same folder: 

    \MODULE_NAME\GUI\SpriteParts\ui_charactercreation\CharacterCreation/Culture\

Then we must create custom brushes to allow the game to use our newly created sprites.

For that we need to copy

    \Mount & Blade II Bannerlord\Modules\Native\GUI\Brushes\CharacterCreation.xml
to

    \OUR_MOD\GUI\Brushes\CharacterCreation.xml

and add our sprites to brushes: "Culture.Banner.Layer.4", "Culture.Banner.Layer.3", "Culture.Banner.Layer.2", "Culture.Banner.Layer.1".

Example for "Culture.Banner.Layer.3":

``` xml
  <Brush Name="Culture.Banner.Layer.3">
    <Layers>
      <BrushLayer Name="Default"  />
    </Layers>
    <Styles>
      <Style Name="baltic">
        <BrushLayer Name="Default" Sprite="CharacterCreation\Culture\baltic_3" />
      </Style>
```

And that will be all to show our custom cultures pictures.

Demo:

<center>
    <iframe width="640" height="360" src="https://www.youtube.com/embed/LRH9zWHL7sY" frameborder="0" allowfullscreen></iframe>
</center>

<br><br>