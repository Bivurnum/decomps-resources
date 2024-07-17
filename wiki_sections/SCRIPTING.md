# Scripting

## Contents
* [Intro](#intro)
* [Poryscript](#poryscript)
* [Constants](#constants)
* [Map Scripts](#map-scripts)
* [Event Scripts](#event-scripts)
* [Movements](#movements)
* [Text](#text)
* [Macros](#macros)

## Intro
Scripts are how we tell the game how to handle most things that happen in the overworld. Each map section has its own file that defines all of the scripts that happen within that area. To find the scripts, navigate to data/maps. Then open the folder of the map you are trying to change the scripts for. The scripts will all be located within the scripts.inc file, which is where we will be doing our editing. (If you are using Poryscript, you will be editing scripts.pory instead.)

This guide will mostly focus on pokeemerald, but the basics still hold true for pokefirered.

## Poryscript
Poryscript is a scripting language that can be incorporated into your decomp project. It allows you to script in a way that more closely resembles the C syntax. I find that it helps me to script more efficiently and concisely. If you'd like to use it, you can find the download, install instructions, and general usage guide here:  
[Poryscript](https://github.com/huderlem/poryscript/blob/master/README.md) (by huderlem)

[(back to top)](#scripting)

## Constants
Let's start at the top of the scripts file. Not every map will have them, but if any constants are defined they will be at the top of the file. Every object event (NPCs, Cut trees, Briney's boat, etc.) is designated an object id number on the map they are in. If you click on an object event in Porymap, it will display its corresponding object id, like so:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/porymap_selected_object.png)

This is the map for Route 110 in Emerald. As you can see, Prof. Birch's object id is 36. The decomp only knows him by this number, so if we want to tell the game to do something with him (like make him walk to the player) we will need to use that number when calling his actions in the scripts.

But 36 different objects is a lot to keep track of on just one map! How is anyone expected to remember which object id number corresponds to which object? It would be tedious to have to switch back and forth between the scripts and Porymap all the time just to check for the number you need.

Well, not to worry! This is where the constants come in. Defining a constant at the top of the file allows you to connect a more user-friendly name to each of the object id numbers you'd like to use in the scripts. Here is an example from the Route 110 scripts:  
```
.set LOCALID_CHALLENGE_BIKER, 21
.set LOCALID_RIVAL, 28
.set LOCALID_RIVAL_ON_BIKE, 29
.set LOCALID_BIRCH, 36
```
The `.set` here tells the decomp that we want to define a new constant. The `LOCALID_...` is the name of the constant you want to define. The number is the object id you want to assign to the constant. Not all object events need to have constants like this, just the ones whose object id numbers you need to use in the scripts file.

> [!NOTE]
> Sometimes `.equ` is used instead of `.set`. As far as I can tell, `.set` is used to define a constant only for use within that file, while `.equ` is used to define a constant that will be referenced in multiple files. On the other hand, pokefirered sometimes uses `.equ` even when that constant is not used in another file, so what do I know?

Now you can use the constant in place of the object id number within this script file. For example, this macro:
```
applymovement 36, Route110_Movement_BirchEntrance
```
That is used much later in the scripts file to tell object number 36 (Prof. Birch) to walk on screen. The macro `applymovement` requires you to give it an object id number, so it knows which object to apply the movement to. We can rewrite the line to this:
```
applymovement LOCALID_BIRCH, Route110_Movement_BirchEntrance
```
Using the constant `LOCALID_BIRCH` instead of the object id number allows anyone who looks at the code (including you) to know exactly which object you are applying the movement to. The decomp still reads it as 36, so nothing has fundamentally changed about the code except our ability to read it easier. This is an extremely convenient functionality to take advantage of. Feel free to define your own constants as needed.

For Poryscript users, the Route 110 constants would look like this:
```
const LOCALID_CHALLENGE_BIKER = 21
const LOCALID_RIVAL = 28
const LOCALID_RIVAL_ON_BIKE = 29
const LOCALID_BIRCH = 36
```
It's in a different format, but it achieves the exact same effect. Just be aware that defining constants in Poryscript like this does not apply to the `raw` sections of code (the original scripting language). In that case you'd have to use `.set` to define constants (like above) in its own `raw` section. It's ok to use both at the same time, like so:
```
const LOCALID_CHALLENGE_BIKER = 21
const LOCALID_RIVAL = 28
const LOCALID_RIVAL_ON_BIKE = 29
const LOCALID_BIRCH = 36

raw `
.set LOCALID_CHALLENGE_BIKER, 21
.set LOCALID_RIVAL, 28
.set LOCALID_RIVAL_ON_BIKE, 29
.set LOCALID_BIRCH, 36
`
```
This allows you to use the constants interchangeably between the poryscript and raw original scripts.

[(back to top)](#scripting)

## Map Scripts
Map scripts are special scripts that run under certain conditions; for example, after you warp into a map or after you close a menu. Every script file is required to have a mapscripts function at the top (below the constants), even if you don't use any map scripts. You cannot have more than one mapscripts function per file. An empty mapscripts function looks like this:
```
Route102_MapScripts::
	.byte 0
```
Or in Poryscript:
```
mapscripts Route102_MapScripts {
}
```
There are 7 different map script types you can use in the mapscripts function, some more useful than others. They are listed here, in order of priority (quoted directly from include/constants/map_scripts.h in the pokeemerald decomp files):
1. ON_DIVE_WARP: Run after the player chooses to dive/emerge. Only used once, to determine whether or not the player should emerge in the Sealed Chamber.
2. ON_TRANSITION: Run during the transition to the map. Used to set map-specific flags/vars, update object positions/movement types, set weather, etc.
3. ON_LOAD: Run after the layout is loaded (but not drawn yet). Almost exclusively used to set metatiles on the map before it's first drawn.
4. ON_RESUME: Run at the end of map load, and again any time upon returning to field (e.g. exiting the Bag menu, or finishing a battle). Used to hide defeated static pokemon, or maintain some map state (e.g. the Trainer Hill timer, or the cycling road challenge). In some maps this takes the metatile setting job of ON_LOAD.
5. ON_RETURN_TO_FIELD: Run exlusively upon returning to the field, shortly after ON_RESUME (as opposed to ON_RESUME, which also runs once on entering the map). Used rarely, when something must only happen on reload (e.g. making sure Mew is above the grass after battling it on Faraway Island).
6. ON_WARP_INTO_MAP_TABLE: Run after the map's objects are loaded. This is a table of scripts; only the first script whose condition is satisfied is run. Used to add objects to the scene or update something about the player as they warp in (e.g. their facing dir or visibility). Note that ON_TRANSITION may also handle object visibility, but would do so by modifying a flag or var.
7. ON_FRAME_TABLE: Run every frame after the map has faded in, before player input is processed. This is a table of scripts; only the first script whose condition is satisfied is run. Used to trigger an event, such as the player exiting the cable car or the SS Tidal sailor announcing progress.

Let's take a look at how they are used in the Route 110 map scripts:
```
Route110_MapScripts::
	map_script MAP_SCRIPT_ON_RESUME, Route110_OnResume
	map_script MAP_SCRIPT_ON_TRANSITION, Route110_OnTransition
	map_script MAP_SCRIPT_ON_FRAME_TABLE, Route110_OnFrame
	.byte 0
```
Notice that the different types of map scripts are not listed in any particular order. It doesn't matter what order you list them inside of the function. The game will always run them in the exact priority order I listed above. In this case, ON_TRANSITION would be run first, then ON_RESUME, then ON_FRAME_TABLE. They are still only run during their specific conditions, though.

After the type of map script is declared, an event script is run farther down the file. ON_TRANSITION runs the script `Route110_OnTransition`, which we can see here:
```
Route110_OnTransition:
	call Common_EventScript_SetupRivalGfxId
	call Common_EventScript_SetupRivalOnBikeGfxId
	call_if_eq VAR_CYCLING_CHALLENGE_STATE, 1, Route110_EventScript_SaveCyclingMusic
	end
```
The first two lines inside of the event script use your player gender to determine whether the rival objects are loaded with Brendan or May graphics. These are appropriate to use in ON_TRANSITION because it is run before the graphics are loaded in. If these had been included in ON_FRAME_TABLE instead, they would've run after the graphics had loaded in and would have had no effect.

Next, let's look at the ON_WARP_INTO_MAP_TABLE map script for Birch's Lab. Here are the map scripts:
```
LittlerootTown_ProfessorBirchsLab_MapScripts::
	map_script MAP_SCRIPT_ON_TRANSITION, LittlerootTown_ProfessorBirchsLab_OnTransition
	map_script MAP_SCRIPT_ON_WARP_INTO_MAP_TABLE, LittlerootTown_ProfessorBirchsLab_OnWarp
	map_script MAP_SCRIPT_ON_FRAME_TABLE, LittlerootTown_ProfessorBirchsLab_OnFrame
	.byte 0
```
ON_WARP_INTO_MAP_TABLE runs the `LittlerootTown_ProfessorBirchsLab_OnWarp` script here:
```
LittlerootTown_ProfessorBirchsLab_OnWarp:
	map_script_2 VAR_BIRCH_LAB_STATE, 2, LittlerootTown_ProfessorBirchsLab_EventScript_SetPlayerPosForReceiveStarter
	map_script_2 VAR_DEX_UPGRADE_JOHTO_STARTER_STATE, 1, LittlerootTown_ProfessorBirchsLab_EventScript_SetObjectPosForDexUpgrade
	map_script_2 VAR_DEX_UPGRADE_JOHTO_STARTER_STATE, 2, LittlerootTown_ProfessorBirchsLab_EventScript_SetObjectPosForDexUpgrade
	map_script_2 VAR_DEX_UPGRADE_JOHTO_STARTER_STATE, 3, LittlerootTown_ProfessorBirchsLab_EventScript_AddRivalObject
	map_script_2 VAR_DEX_UPGRADE_JOHTO_STARTER_STATE, 6, LittlerootTown_ProfessorBirchsLab_EventScript_AddRivalObject
	map_script_2 VAR_DEX_UPGRADE_JOHTO_STARTER_STATE, 4, LittlerootTown_ProfessorBirchsLab_EventScript_SetObjectPosForJohtoStarters
	map_script_2 VAR_DEX_UPGRADE_JOHTO_STARTER_STATE, 5, LittlerootTown_ProfessorBirchsLab_EventScript_SetObjectPosForJohtoStarters
	.2byte 0
```
At first glance, it looks like there is a lot going on here. However, all of the map script types that end in `_TABLE` only ever run one line. In this case, when the player warps into the map, ON_WARP_INTO_MAP_TABLE will go through these lines one by one until it finds a condition that is true and then ONLY run that line. For example:
```
map_script_2 VAR_BIRCH_LAB_STATE, 2, LittlerootTown_ProfessorBirchsLab_EventScript_SetPlayerPosForReceiveStarter
```
This line states that if the variable `VAR_BIRCH_LAB_STATE` is currently equal to exactly 2, the code should run the script `LittlerootTown_ProfessorBirchsLab_EventScript_SetPlayerPosForReceiveStarter`. If `VAR_BIRCH_LAB_STATE` happens to be equal to 1 at that time, ON_WARP_INTO_MAP_TABLE will skip that line entirely and move on to checking the next line. Once a line's condition is found to be true, that corresponding script is run and the rest of the lines are ignored.

It is important to note that the script associated with ON_WARP_INTO_MAP_TABLE ends in `.2byte 0`. All `_TABLE` map scripts need to end with that. The mapscripts function itself needs to end in `.byte 0`. Don't ask me why. It's just the way the assembly language handles map scripts. One of the conveniences of using Poryscript is that you never have to worry about that; it just adds the appropriate line in for you automatically.

The point of using map scripts is to take advantage of the specific conditions in which they run. They can take a bit of getting used to, but you'll get the hang of them the more you try to use them. Look at how the different maps handle the existing map scripts to get a better idea of how they are used.

Poryscript handles the syntax of map scripts a bit differently. Consult [this section](https://github.com/huderlem/poryscript/blob/master/README.md#mapscripts-statement) of the Poryscript guide for further information.

[(back to top)](#scripting)

## Event Scripts

[(back to top)](#scripting)

## Movements

[(back to top)](#scripting)

## Text

[(back to top)](#scripting)

## Macros
A macro is like a mini function.

[(back to top)](#scripting)
