#Easily Toggle Different Wild Encounters

_written by Bivurnum_  
_works with pokeemerald-expansion versions 1.8.0 through 1.9.0 (and likely some older versions)_

This easy edit will allow you to change the wild encounter tables for maps with a single variable.

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/gifs/encounter_table_example.gif)

First, choose the variable you want to use. In include/constants/vars.h, choose one of the unused variables and rename it to `VAR_ENCOUNTER_TABLE`. For example:
![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/unused_var-example.png)  
`VAR_UNUSED_0x404E` is an appropriate one to rename, but you can use any var that is labeled as unused.

Now, go to src/wild_encounter.c and add the line `i += VarGet(VAR_ENCOUNTER_TABLE);` to the `GetCurrentMapWildMonHeaderId` function in the location pictured here:
![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/add_to_GetCurrentMapWildMonHeaderId.png)

That's all there is to it!

## How to Use it
The wild encounter tables will changed based on the current value of `VAR_ENCOUNTER_TABLE`. For example, `VAR_ENCOUNTER_TABLE` is set to 0 by default, which makes wild encounters use the first encounter table. If you changed the value of `VAR_ENCOUNTER_TABLE` to 1, it would change the wild Pokemon to the first additional encounter table. You can add as many additional encounter tables as you want for this.

When you make a new encounter table in Porymap, it automatically generates a name for your new encounter table with an extra number on the end. Conveniently, this number in the name naturally corresponds with the value of `VAR_ENCOUNTER_TABLE` that is required to use it!

Note:  
Keep in mind that this is kind of a quick and dirty implementation, so be aware that if `VAR_ENCOUNTER_TABLE` is set to 1 and the current map does not have an additional encounter table, it will use the encounter table for a completely different map. You have to make sure the variable never exceeds the number of encounter tables for the player's current location. This is avoided if all maps have the same number of encounter tables.
