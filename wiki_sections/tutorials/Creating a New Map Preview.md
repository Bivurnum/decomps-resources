# Creating a New Map Preview

This guide will show you how to take a custom image and turn it into a map preview for use with my [frlg-map-previews](https://github.com/Bivurnum/decomps-resources/wiki/FRLG-Map-Previews) feature branch for pokeemerald projects. The instructions given here are specific to my feature, but a lot of what I cover can be generally useful. I'll be as comprehensive as I can with the relevant mechanics. I also provide example images from both Aseprite and Graphics Gale for the convenience of more users.

## Contents
* [The Image](#the-image)
* [The Tilemap](#the-tilemap)
* [The Palettes](#the-palettes)
   * [Fade In](#fade-in)
   * [No Fade](#no-fade)
* [Map Name Text](#map-name-text)
* [The Code](#the-code)

# The Image
I'm not much of an artist myself, so I will leave the design of the image to you. For this demonstration, I have picked out a lovely [preview image](https://bulbapedia.bulbagarden.net/wiki/Location_preview#/media/File:HGSS_Ruins_of_Alph-Day.png) from the HGSS games:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/HGSS_Ruins_of_Alph-Day.png)

The GBA display is exactly 240x160 pixels, so if your base image is larger than that you will have to crop it to fit. My image is a little too wide, so I'll have to trim it down. I also want to incorporate the cinematic black bars, like they used in the FRLG games, so I'll have to trim quite a bit off of the top and bottom as well.

I have created a template image that is an exact copy of the FRLG map preview, including the black bars and text box. You can find the image in the ***graphics/map_previews*** folder. Feel free to use it for your own previews:

![](https://github.com/Pawkkie/Team-Aquas-Asset-Repo/blob/main/Other/Bivurnum/map_preview_template.png)

Now I have my map preview exactly how I want it to look in game:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/alph_map_preview_image.png)

You're probably wondering why there is a big yellow bar along the right side of the image. Those extra tiles are needed because the game will load some tiles off screen. Without the yellow bar taking up that space, some of our map preview tiles would load there instead, screwing up the image. So while the GBA display is only 240x160, all map preview images must be 256x160 pixels in size. Don't worry. Those extra tiles won't show up in the game. (So you can make them whatever color you want. They don't have to be yellow.)

Finally, you need to make sure that the image is properly indexed.  
<details>
    <summary><i>How to index an image in Aseprite:</i></summary>

>   You just need to click `Sprite`, then `Color Mode`, then make sure the `Indexed` option is checked.
>   
>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/aseprite_index_example.png)
>   
>   Make sure to save the image when you are done.
</details>
<details>
    <summary><i>How to index an image in Graphics Gale:</i></summary>

>   You need to click `All Frames`, then `Color Depth...`.
>   
>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/gale_color_depth.png)
>   
>   Because map preview images use more than one 16-color palette, you should select the `8bpp` option. Then click `OK`.
>   
>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/gale_8bpp.png)
>   
>   Make sure to save the image when you are done.
</details>

# The Tilemap
Now that we have our image, we need to covert it into a tile sheet and generate a tilemap using [Tilemap Studio](https://github.com/Rangi42/tilemap-studio/blob/master/README.md).
<details>
    <summary><i>A note to Mac users:</i></summary>

>   While there are experimental versions of Tilemap Studio for Mac, the Image to Tiles feature used in this guide does not currently work properly on a Mac.  
>   Feel free to contact me on Discord (@Bivurnum) and I will help you generate a tile sheet and tilemap myself if I am available to do so.
</details>

In Tilemap Studio, we want to use the Image to Tiles feature.  
Click on `Tools`, then `Image to Tiles...`. Alternatively, you could also click on the little orange portrait icon on the far right, shown in the image below.

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/TS_image_to_tiles.png)

This window will pop up:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/image_to_tiles_window.png)

For the `Input` and `Output` options, click where it says `No file selected` and select the proper files.
* The `Input` should be the png image you just created.
* The `Output` should be the location you want the tile sheet and tilemap files to be generated. I recommend making a new folder for your map inside of the ***graphics/map_previews*** folder and setting the `Output` there. The name of the new files can be whatever you want, but if you want to match what is used for the vanilla map previews, I recommend setting the name to "tiles".

If you want, you can check the box that says `Avoid extra blank tiles at the end`. That will make the tile sheet only contain tiles that are used in the image, saving a tiny bit of space. However, it is not strictly necessary and I will not be using it in this example. Just keep in mind that it might alter the dimensions of the tiles image to be different than my example, but it will still work properly. Leaving it unchecked might just include a handful of blank tiles at the bottom of the image. No big deal.

The Tilemap `Format` should be set to `GBA tiles + 4bpp palettes`.

Check the box for `Blank tiles use ID` and set its value to `000`.

The Palette `Format` should be set to `Indexed in tileset image`.

Check the box for `Color 0` and choose what color you want to use for transparency in all of the palettes. The transparent color won't be used in the visible part of the image, so it doesn't really matter. I like to use the color of the tiles that will be off screen in order to minimize the number of unique colors required in the palettes. (The yellow color is `FFFF00`)

My Image to Tiles window looks like this now:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/image_to_tiles_window_filled.png)

Now click `OK`. You should get a little alert telling you it was a success. If you get an error about too many palettes, you may have to edit your initial image to remove some unique colors or redraw parts of the image so the multiple palettes have to share less colors between them in order to transition smoothly from one palette to another.

Now it will display your new tile sheet and tilemap:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/TS_tiles.png)

# The Palettes
You may have noticed that the colors of some of the tiles are not correct. This is FINE. The current version of Tilemap Studio (4.0.1) sets all of the pixels in the tiles.png file to use the first palette for whatever reason. However, as long as you retain the order of the colors in each palette in the png, the game will assign them the correct palette and the map preview will show up with the proper colors.

If you use `MPS_TYPE_FADE_IN` as the map preview behavior, only three palettes can be loaded into the game for the tiles.png image to utilize. Otherwise, the map preview can take advantage of all 16 palette slots.

An overview of the map preview behavior types can be found [here](https://github.com/Bivurnum/decomps-resources/wiki/FRLG-Map-Previews#type).  
There are also visual examples of each type at the [bottom of this guide](#mps-type-examples).

This is where the instructions diverge based on what behavior you want your map preview to have in game.
* If your map preview will use `MPS_TYPE_FADE_IN`, follow the instructions in the [Fade In](#fade-in) section.
* If your map preview will use `MPS_TYPE_BASIC` or `MPS_TYPE_CAVE`, skip to the [No Fade](#no-fade) section and follow the instructions there instead.

## Fade In
Because the map preview will fade into the overworld map, the map preview palettes will be loaded into palette slots that don't interfere with the overworld palettes. Specifically, they will be loaded into slots `D`, `E`, and `F`. So, we need to change the palettes in the tilemap to match. If you open the new tiles.png file in your image editor, you can see how many palettes the map preview ended up with:

<details>
    <summary><i>Aseprite:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/aseprite_2_palettes.png)
</details>
<details>
    <summary><i>Graphics Gale:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/gale_2_palettes.png)
</details>

My image ended up with exactly two palettes, which is an ideal situation. I'm going to move the palettes around so none of them get loaded into palette slot `E`. Palette slot `E` is reserved specifically for the colors used for the text that displays the map name.  
For now, we just want to leave the second palette empty, like so:

<details>
    <summary><i>Aseprite:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/aseprite_3_palettes.png)
</details>
<details>
    <summary><i>Graphics Gale:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/gale_3_palettes.png)
</details>

Remember that you can move whole 16-color palettes to different positions in the png, but you should not rearrange the order of the colors within any individual 16-color palette. The Image to Tiles function sets the colors to very specific positions and you don't want to mess around with that unless you really know what you are doing. It's a pain in the butt!  
As long as you follow what I do in my screenshots, you should be fine.

~ ~ ~

Now go back to Tilemap Studio and click on the `Palettes` tab.  
Here you can see what tiles will use which palettes:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/TS_palettes_initial.png)

We need to change the palette numbers in the tilemap to match the palette slots they will be loaded into. The tiles marked `0` should be changed to `D`. The ones marked `1` should be changed to `F`. Shift clicking acts as a fill tool.  
Here is what my tilemap looks like after the change:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/TS_palettes_fade_in.png)

If your tiles.png file only has one palette, just change the whole tilemap to the `D` palette.

If your tiles.png file ended up with more than two palettes, you may have to edit your initial image to remove some unique colors or redraw parts of the image so that multiple palettes have to share fewer colors between them in order to transition smoothly from one palette to another. If you are skilled enough (or masochistic enough) you can try to rearrange some colors so they can be included in the `E` palette. I'll go over the `E` palette in more detail in the [Map Name Text](#map-name-text) section later.

If your tiles.png file ended up with A LOT more than two palettes, you should consider using the `MPS_TYPE_BASIC` behavior for the map preview instead. (If so, click [here](#no-fade) to go to the [No Fade](#no-fade) section.)

Make sure to save your changes.

The following No Fade section should be skipped if your map preview uses `MPS_TYPE_FADE_IN`. **[CLICK HERE](#map-name-text) to go to the next relevant section.**

## No Fade
If you open the new tiles.png file in your image editor, you can see how many palettes the map preview ended up with:

<details>
    <summary><i>Aseprite:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/aseprite_2_palettes.png)
</details>
<details>
    <summary><i>Graphics Gale:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/gale_2_palettes.png)
</details>

If your image has 14 palettes or less, you can [CLICK HERE](#map-name-text) to move to the next section. My image ended up with two palettes, which is a small enough number that I wouldn't have to do anything else in this section.

If your image has 15 or 16 palettes, continue with this section.

~ ~ ~

I'm going to show you a different example that ended up with 15 palettes:

<details>
    <summary><i>Aseprite:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/aseprite_15_palettes.png)
</details>
<details>
    <summary><i>Graphics Gale:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/gale_15_palettes.png)
</details>

I'm going to move the palettes around so none of them get loaded into palette slot `E`. Palette slot `E` is reserved specifically for the colors used for the text that displays the map name. For now, we just want to leave the 15<sup>th</sup> palette empty, like so:

<details>
    <summary><i>Aseprite:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/aseprite_16_palettes.png)
</details>
<details>
    <summary><i>Graphics Gale:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/gale_16_palettes.png)
</details>

Remember that you can move whole 16-color palettes to different positions in the png, but you should not rearrange the order of the colors within any individual 16-color palette. The Image to Tiles function sets the colors to very specific positions and you don't want to mess around with that unless you really know what you are doing. It's a pain in the butt!  
As long as you follow what I do in my screenshots, you should be fine.

If your tiles.png file ended up with 16 palettes, you may have to edit your initial image to remove some unique colors or redraw parts of the image so that multiple palettes have to share fewer colors between them in order to transition smoothly from one palette to another. If you are skilled enough (or masochistic enough) you can try to rearrange some colors so they can be included in the `E` palette. I'll go over the `E` palette in more detail in the [Map Name Text](#map-name-text) section next.

Make sure to save your changes.

~ ~ ~

Now go back to Tilemap Studio and click on the `Palettes` tab. Here you can see what tiles will use which palettes:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/TS_palettes_15.png)

We need to change the palette numbers in the tilemap to match where we moved them to in the png. Since we moved a palette from the 15<sup>th</sup> position to the 16<sup>th</sup> position, the tiles marked `E` should be changed to `F`. Shift clicking acts as a fill tool. CTRL clicking a tile will replace every tile of that palette with the selected palette.  
Here is what my tilemap looks like after the change:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/TS_palettes_16.png)

Make sure to save your changes.

# Map Name Text
The current map's name will be printed in a text box in the upper left corner of the screen. The FRLG template I used earlier shows the exact area of the image that the text box will take up (104x24 pixels, directly in the corner).

If the map's name is longer than the standard text box size, the text box's width will automatically be extended by an extra 72 pixels. I have another FRLG template image that conforms to this larger size. You can find it in the ***graphics/map_previews*** folder.

The map name being loaded in separately from the image allows you to use the same map preview image for multiple different maps.

~ ~ ~

Palette `E`, the one that is loaded into the second to last palette slot, is the one that the game uses for text and text boxes (not to be confused with the borders around the text boxes). While the palette is comprised of 16 colors, we are only concerned with 3 of them:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/numbered_palette.png)

* Color `1` (Red): Used for the text box itself. This is what the text gets printed on top of.
* Color `3` (Green): Used for the shadow of the characters of text.
* Color `4` (Blue): Used for the text itself.

The positions of these colors in the palette are specific to these functions and cannot be changed.

To demonstrate this in action, here is what the map name text looks like with the colors used in FRLG. The yellow colors in the palettes are filler:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/map_name_frlg_pal.png)

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/map_name_frlg.png)

And here is the same map name text, but with the colors drastically changed:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/map_name_vivid_pal.png)

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/map_name_vivid.png)

The rest of the colors in this palette can be used like normal (other than the transparent color `0`). This means you can utilize the unused portion as a sort of partial 16<sup>th</sup> palette for your map preview image. This takes further skill and knowledge than I will be covering in this guide, but if you can get the colors to line up with the pixels properly, you'll have that much more versatility with the image.

Keep in mind that this palette needs to be in the 15<sup>th</sup> slot of tiles.png, unless you are using `MPS_TYPE_FADE_IN`. In that case, you should put it in the second slot instead.

Here is what the final palettes will look like in my Ruins of Alph tiles.png image if I use `MPS_TYPE_FADE_IN`:

<details>
    <summary><i>Aseprite:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/aseprite_complete_pal_fade.png)
</details>
<details>
    <summary><i>Graphics Gale:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/gale_complete_pal_fade.png)
</details>

Here is what it will look like if I use `MPS_TYPE_BASIC` or `MPS_TYPE_CAVE`:

<details>
    <summary><i>Aseprite:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/aseprite_complete_pal.png)
</details>
<details>
    <summary><i>Graphics Gale:</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/create_a_map_preview/gale_complete_pal.png)
</details>

Once again, make sure to save your changes.

# The Code
Now that we have our files all sorted, it is time to incorporate our new map preview into the code.

First, if you want to adhere to the naming conventions of the other map previews, you can rename the tiles.bin file to tilemap.bin. This isn't strictly necessary, but I find it convenient to keep all of the file names consistent.

Open ***include/map_preview_screen.h*** and add an entry for your new map preview image in the `PreviewImageId` enum, right before `IMG_COUNT`. Name it something unique and representative of the image. I'm adding the preview from HGSS's Ruins of Alph, so I'll name it `IMG_RUINS_OF_ALPH`:

```diff
enum PreviewImageId
{
    IMG_VIRIDIAN_FOREST,
    IMG_MT_MOON,
    IMG_DIGLETTS_CAVE,
    IMG_ROCK_TUNNEL,
    IMG_POKEMON_TOWER,
    IMG_SAFARI_ZONE,
    IMG_SEAFOAM_ISLANDS,
    IMG_POKEMON_MANSION,
    IMG_ROCKET_HIDEOUT,
    IMG_SILPH_CO,
    IMG_VICTORY_ROAD,
    IMG_CERULEAN_CAVE,
    IMG_POWER_PLANT,
    IMG_MT_EMBER,
    IMG_ROCKET_WAREHOUSE,
    IMG_MONEAN_CHAMBER,
    IMG_DOTTED_HOLE,
    IMG_BERRY_FOREST,
    IMG_ICEFALL_CAVE,
    IMG_LOST_CAVE,
    IMG_ALTERING_CAVE,
+   IMG_RUINS_OF_ALPH,
    IMG_COUNT
};
```

Now go to ***src/map_preview_screen.c*** and add filepaths to your new files. You'll need to add three new lines. One for the palette, one for the tiles image, and one for the tilemap. The palette file does not exist yet, but this line of code will tell the compiler to generate a gbapal file from the tiles image. You'll also need to add .4bpp.lz and .bin.lz suffixes to the tiles and tilemap filepaths respectively. That tells the compiler to generate versions of those files that the GBA can read.  
Here's how I did mine:

```diff
static const u8 sIcefallCaveMapPreviewPalette[] = INCBIN_U8("graphics/map_preview/icefall_cave/tiles.gbapal");
static const u8 sIcefallCaveMapPreviewTiles[] = INCBIN_U8("graphics/map_preview/icefall_cave/tiles.4bpp.lz");
static const u8 sIcefallCaveMapPreviewTilemap[] = INCBIN_U8("graphics/map_preview/icefall_cave/tilemap.bin.lz");
static const u8 sAlteringCaveMapPreviewPalette[] = INCBIN_U8("graphics/map_preview/altering_cave/tiles.gbapal");
static const u8 sAlteringCaveMapPreviewTiles[] = INCBIN_U8("graphics/map_preview/altering_cave/tiles.4bpp.lz");
static const u8 sAlteringCaveMapPreviewTilemap[] = INCBIN_U8("graphics/map_preview/altering_cave/tilemap.bin.lz");
+static const u8 sRuinsOfAlphMapPreviewPalette[] = INCBIN_U8("graphics/map_preview/ruins_of_alph/tiles.gbapal");
+static const u8 sRuinsOfAlphMapPreviewTiles[] = INCBIN_U8("graphics/map_preview/ruins_of_alph/tiles.4bpp.lz");
+static const u8 sRuinsOfAlphMapPreviewTilemap[] = INCBIN_U8("graphics/map_preview/ruins_of_alph/tilemap.bin.lz");
```

Again, the names of things can be whatever you want, but keeping with the existing name convention can help you stay organized and informed. For example, I could replace `sRuinsOfAlphMapPreviewTilemap` with `SmittyWerbenJaegerManJensen` and it would still work just fine. But `sRuinsOfAlphMapPreviewTilemap` is so much more descriptive, I never run the risk of forgetting what its intended use is. Do what feels right to you.

Further down in that same file, you'll need to add a new entry to the VERY END of the `sMapPreviewImageData` struct. The name of the entry (in brackets []) should be the name that you added to the `PreviewImageId` enum in ***map_preview_screen.h***. In my case, it would be `IMG_RUINS_OF_ALPH`. Then you need to list the names of the filepaths we added above. Reference the other entries as needed to get the format right.  
Here's how I added my `IMG_RUINS_OF_ALPH` entry:

```diff
static const struct ImageData sMapPreviewImageData[IMG_COUNT] = {
    [IMG_VIRIDIAN_FOREST] = {
        .tilesptr = sViridianForestMapPreviewTiles,
        .tilemapptr = sViridianForestMapPreviewTilemap,
        .palptr = sViridianForestMapPreviewPalette
    },

...
Lots of entries.
...

    [IMG_ALTERING_CAVE] = {
        .tilesptr = sAlteringCaveMapPreviewTiles,
        .tilemapptr = sAlteringCaveMapPreviewTilemap,
        .palptr = sAlteringCaveMapPreviewPalette
-   }
+   },
+   [IMG_RUINS_OF_ALPH] = {
+       .tilesptr = sRuinsOfAlphMapPreviewTiles,
+       .tilemapptr = sRuinsOfAlphMapPreviewTilemap,
+       .palptr = sRuinsOfAlphMapPreviewPalette
+   }
};
```

That's it! Now your new map preview has been added to the game!

Follow the instructions [HERE](https://github.com/Bivurnum/decomps-resources/wiki/FRLG-Map-Previews#using-the-map-previews) to link the map preview to a map.

Here are some in-game examples of my new Ruins of Alph map preview with each behavior type:
<a name="mps-type-examples"></a>

<details>
    <summary><i>MPS_FADE_IN</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/gifs/create_a_map_preview/ruins_of_alph_fade.gif)
</details>
<details>
    <summary><i>MPS_BASIC</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/gifs/create_a_map_preview/ruins_of_alph_basic.gif)
</details>
<details>
    <summary><i>MPS_CAVE</i></summary>

>   ![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/gifs/create_a_map_preview/ruins_of_alph_cave.gif)
</details>
