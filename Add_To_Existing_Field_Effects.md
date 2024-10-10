# Adding Additional Images/Palettes to Existing Field Effects

This brief guide shows how you could add new graphics to the existing field effects in order to change the images/palettes based on the criteria you set.
The field effect system is convoluted and hard to add to, so it is much easier to piggyback off of the existing code than it is to add an entirely new field effect.
A more comprehensive version of this guide will be added eventually.

The example used in some of the steps below are for changing the image and palette of the step in tall grass field effect only while the player is in the Route 102 map, but still using the default graphics in all other maps.
However, I imagine you could use similar steps in order to achieve similar results with most (if not all) of the other field effects in the game.

Some use case examples for this would be:
* if you wanted to change the palette of the tall grass effect for a custom seasons system, (ie, a palette of whites during winter, or a palette of browns during autumn, etc).
* if you wanted to use different tall grass metatiles for different areas of your region, you could use `MB_TALL_GRASS` for the behavior of all of them and toggle the graphics used based on the player's current location.

### Steps
1) Add filepaths to all new images and palettes to `src/data/object_events/object_event_graphics.h`.

2) Add a new entry for each new image to `spritesheet_rules.mk`.

3) In `include/constants/field_effects.h`, add a `FLDEFF_` constant for each new palette and a new `FLDEFFOBJ_` constant for each new image.

4) In `src/data/field_effects/field_effect_objects.h`, add a new `gSpritePalette_` entry for each new palette (use `FLDEFF_PAL_TAG_GENERAL_1` for the pal tag). For each new image, add a new `sPicTable_` entry and a new `gFieldEffectObjectTemplate_` entry.

5) In `src/data/field_effects/field_effect_object_template_pointers.h`, add a new `extern const struct SpriteTemplate` line for each new `gFieldEffectObjectTemplate_` entry you just made. Then tie the new `FLDEFFOBJ_` entries to the corresponding new `gFieldEffectObjectTemplate_` entries in the `gFieldEffectObjectTemplatePointers` table at the end of the file.

6) In `data/field_effect_scripts.s`, for each new palette, add a new `.4byte gFieldEffectScript_` to the list. This MUST be in the same order as the `FLDEFF_` constants are defined in `include/constants/field_effects.h`. Then add a new function for each new `gFieldEffectScript_`, using the corresponding `gSpritePalette_` entries you added earlier.

7) In `src/event_object_movement.c`, add new `if` logic in both the `GroundEffect_SpawnOnTallGrass` and `GroundEffect_StepOnTallGrass` functions for each new palette you want to use. The new `if logic needs to determine which palette it needs to use based on whatever criteria you want to use. For example, this logic uses a different palette for the tall grass effect only on Route 102:
```
    if (gSaveBlock1Ptr->location.mapNum == MAP_NUM(ROUTE102) && gSaveBlock1Ptr->location.mapGroup == MAP_GROUP(ROUTE102))
    {
        FieldEffectStart(FLDEFF_TALL_GRASS_2);
    }
    else
    {
        FieldEffectStart(FLDEFF_TALL_GRASS);
    }
```

8) In `src/field_effect_helpers.c`, add new `if` logic to the `FldEff_TallGrass` function. This new `if` logic needs to determine which image to use for the effect based on whatever criteria you want to use. For example, this logic uses a different image for the tall grass effect only on Route 102:
```
    if (gSaveBlock1Ptr->location.mapNum == MAP_NUM(ROUTE102) && gSaveBlock1Ptr->location.mapGroup == MAP_GROUP(ROUTE102))
    {
        spriteId = CreateSpriteAtEnd(gFieldEffectObjectTemplatePointers[FLDEFFOBJ_TALL_GRASS_2], x, y, 0);
    }
    else
    {
        spriteId = CreateSpriteAtEnd(gFieldEffectObjectTemplatePointers[FLDEFFOBJ_TALL_GRASS], x, y, 0);
    }
```

9) Also in `src/field_effect_helpers.c`, you will need to add new `if` logic to the `UpdateTallGrassFieldEffect` funtion. This `if` logic needs to tell the game which effect is currently being used based on the exact logic you specified in `src/event_object_movement.c`. For example, this logic follows the same criteria that was used in the earlier examples:
```
        if (gSaveBlock1Ptr->location.mapNum == MAP_NUM(ROUTE102) && gSaveBlock1Ptr->location.mapGroup == MAP_GROUP(ROUTE102))
        {
            FieldEffectStop(sprite, FLDEFF_TALL_GRASS_2);
        }
        else
        {
            FieldEffectStop(sprite, FLDEFF_TALL_GRASS);
        }
