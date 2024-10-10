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
