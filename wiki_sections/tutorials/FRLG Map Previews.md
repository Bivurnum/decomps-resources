# FRLG Map Previews

<a name="mps-fade-in-gif"></a>

![map_preview_example](https://github.com/Bivurnum/decomps-resources/blob/main/assets/gifs/map_preview_example.gif)

I implemented FireRed's map preview screen feature in pokeemerald and pokeemerald-expansion. This will allow you to use these map preview screens for whatever maps you want. Incidentally, I made this branch entirely from scratch before I found out that [ghoulslash had already done it](https://www.pokecommunity.com/threads/map-previews.435664/) years ago! Our code is very similar, mostly because the majority of it is just directly ported from pokefirered. Full credit to ghoulslash for beating me to the punch! However, I did make some tweaks to the code to make it generally easier for most people to use.

<details>
    <summary><i>Basic merge instructions:</i></summary>

>   The repo can be found here: [https://github.com/Bivurnum/pokeemerald/tree/map-previews](https://github.com/Bivurnum/pokeemerald/tree/map-previews)
>
>   Enter the following commands into the console:  
>   `git remote add Bivurnum https://github.com/Bivurnum/pokeemerald`  
>   `git pull Bivurnum map-previews`
>
>   Or, if you want to apply the changes manually, you can find the comparison with all of my changes here:  
>   [https://github.com/Bivurnum/pokeemerald/compare/master...map-previews](https://github.com/Bivurnum/pokeemerald/compare/master...map-previews)
</details>

<details>
    <summary><i>Merge instructions for expansion 1.9 and 1.10:</i></summary>

>   The repo can be found here: [https://github.com/Bivurnum/pokeemerald-expansion/tree/map-previews](https://github.com/Bivurnum/pokeemerald-expansion/tree/map-previews)
>
>   Enter the following commands into the console:  
>   `git remote add Biv-expansion https://github.com/Bivurnum/pokeemerald-expansion`  
>   `git pull Biv-expansion map-previews`
>
>   Or, if you want to apply the changes manually, you can find the comparison with all of my changes here:  
>   [https://github.com/rh-hideout/pokeemerald-expansion/compare/expansion/1.9.0...Bivurnum:pokeemerald-expansion:map-previews](https://github.com/rh-hideout/pokeemerald-expansion/compare/expansion/1.9.0...Bivurnum:pokeemerald-expansion:map-previews)
</details>

Click [here](https://github.com/Bivurnum/decomps-resources/wiki/Creating-a-New-Map-Preview) for a guide on how to create your own map previews.

## Using the Map Previews
I'll be using an example of adding a map preview to Petalburg Woods for this section.

You will need to define a new map preview screen in ***include/map_preview_screen.h***. Just add your new entry near the bottom of the list in `MapPreviewScreenId`, leaving `MPS_COUNT` at the very end. Like so:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/MapPreviewScreenId_example.png)

Then, you will go to ***src/map_preview_screen.c*** and add a new section in the `sMapPreviewScreenData` list for the map preview you just defined. Keep in mind that this list has to be in the same order as the list in ***include/map_preview_screen.h***.

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/sMapPreviewScreenData_example.png)

* `mapsec` is just the name of the map section your map is in. This map preview will apply to all maps within this map section (but not when traveling between maps within the same map section, similarly to the map name popups). If it doesn't have one alredy, the mapsec will need an entry in ***src/data/region_map/region_map_sections.json*** for the map name to show up in the map preview.
<a name="type"></a>
* `type` will be either `MPS_TYPE_BASIC`, `MPS_TYPE_FADE_IN` or `MPS_TYPE_CAVE`.
  * `MPS_TYPE_BASIC` will show the map preview, then fade to black before loading into the new map.
  * `MPS_TYPE_FADE_IN` will fade the map preview into the new map (see the [gif at the top of this page](#mps-fade-in-gif) for example).
  * `MPS_TYPE_CAVE` will perform the warp animation for entering a cave at the end of the map preview, before the map is loaded in.
<details>
    <summary><i>MPS_BASIC example</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/gifs/create_a_map_preview/ruins_of_alph_basic.gif)
</details>
<details>
    <summary><i>MPS_FADE_IN example</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/gifs/create_a_map_preview/ruins_of_alph_fade.gif)
</details>
<details>
    <summary><i>MPS_CAVE example</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/gifs/create_a_map_preview/ruins_of_alph_cave.gif)
</details>

* `flagId` is just the name of the flag you want to set when visiting this map for the first time. The flag state determines how long the map preview will last on screen; it gets a shorter duration if the player has been to that map before. A lot of Emerald maps already have associated flags, but Petalburg Woods does not have one by default, so I repurposed one of the unused flags for use in this example. If you don't want to use a flag, you can enter `MPS_FLAG_NULL` here and the duration will have its own custom value (this can be adjusted in the configs in ***include/map_preview_screen.h***).
* `image` is the image you'd like to use for the preview screen. The full list of options can be found in ***include/map_preview_screen.h*** under `PreviewImageId`.

That's all there is to it!

### Scripting
Two new scripting macros have been added:
* `mappreview` fades the screen to black, runs a map preview for the current map, then fades back from black before continuing the script. This uses the behavior of `MPS_TYPE_BASIC`, regardless of the designated type of the map.
* `mappopup` initiates the vanilla map name popup for the current map.

### Configs
In the ***include/map_preview_screen.h*** file, there are a few configs you can customize for the map previews.
* `MPS_DURATION_LONG` determines how many frames the map preview lasts on screen the first time the player enters that map.
* `MPS_DURATION_SHORT` determines how many frames the map preview lasts on screen after the map's flag has been set.
* `MPS_DURATION_NO_FLAG` determines how many frames the map preview lasts if the flag has been set to `MPS_FLAG_NULL`.
* `MPS_DURATION_ALWAYS` overrides all other duration values if it is set to anything but 0.
* `MPS_DURATION_SCRIPT` determines how many frames the map preview lasts when triggered by `mappreview`. It is not affected by `MPS_DURATION_ALWAYS`.
* `MPS_BASIC_FADE_SPEED` determines how quickly the fade to black animation plays at the end of `MPS_TYPE_BASIC`. A larger value makes it fade out slower. A smaller value (even a negative number) makes it fade out faster.

### Notes
* Keep in mind that ON_FRAME mapscripts may not play nice with the `MPS_TYPE_FADE_IN` map previews due to the mapscript running before the map preview screen has fully faded out. You could use `MPS_TYPE_BASIC` or the `mappreview` script macro to get around this.
* `MPS_TYPE_FADE_IN` does not work properly if the map has any of the following weather types. (You can use `MPS_TYPE_BASIC` for the map preview instead.)
   * `WEATHER_SUNNY_CLOUDS`
   * `WEATHER_FOG_HORIZONTAL`
   * `WEATHER_FOG_DIAGONAL`
   * `WEATHER_UNDERWATER`
   * `WEATHER_UNDERWATER_BUBBLES`
