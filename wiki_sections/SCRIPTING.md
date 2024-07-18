# Scripting

## Contents
* [Intro](#intro)
* [Poryscript](#poryscript)
* [Constants](#constants)
* [Map Scripts](#map-scripts)
* [Event Scripts](#event-scripts)
* [Movements](#movements)
* [Text](#text)
* [All Together](#all-together)
* [Macros](#macros)

## Intro
Scripts are how we tell the game how to handle most things that happen in the overworld. Each map section has its own file that defines all of the scripts that happen within that area. To find the scripts, navigate to data/maps. Then open the folder of the map you are trying to change the scripts for. The scripts will all be located within the scripts.inc file, which is where we will be doing our editing. (If you are using Poryscript, you will be editing scripts.pory instead.)

This guide will mostly focus on pokeemerald, but the basics still hold true for pokefirered.

## Poryscript
Poryscript is a scripting language that can be incorporated into your decomp project. It allows you to script in a way that more closely resembles the C syntax. I find that it helps me to script more efficiently and concisely. It is highly recommended! If you'd like to use it, you can find the download, install instructions, and general usage guide here:  
[Poryscript](https://github.com/huderlem/poryscript/blob/master/README.md) (by huderlem)

[(back to top)](#scripting)

## Constants
Let's start at the top of the scripts file. Not every map will have them, but if any constants are defined they will be at the top of the file. Every object (NPCs, Cut trees, Briney's boat, etc.) is designated an object id number on the map they are in. If you click on an object in Porymap, it will display its corresponding object id, like so:

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
The `.set` here tells the decomp that we want to define a new constant. The `LOCALID_...` is the name you give to the constant (this can be whatever you want). The number is the object id you want to assign to the constant. Not all objects need to have constants like this, just the ones whose object id numbers you need to use in the scripts file.

> [!NOTE]
> Sometimes `.equ` is used instead of `.set`. As far as I can tell, `.set` is used to define a constant only for use within that file, while `.equ` is used to define a constant that will be referenced in multiple files. On the other hand, pokefirered sometimes uses `.equ` even when that constant is not used in another file, so what do I know?

Now you can use the constant in place of the object id number within this script file. For example, this macro:
```
applymovement 36, Route110_Movement_BirchEntrance
```
This is used much later in the scripts file to tell object number 36 (Prof. Birch) to walk on screen. The macro `applymovement` requires you to give it an object id number, so it knows which object to apply the movement to. We can rewrite the line to this:
```
applymovement LOCALID_BIRCH, Route110_Movement_BirchEntrance
```
Using the constant `LOCALID_BIRCH` instead of the object id number allows anyone who looks at the code (including you) to know at a glance exactly which object you are applying the movement to. The decomp still reads it as 36, so nothing has fundamentally changed about the code except our ability to read it easier. This is an extremely convenient functionality to take advantage of. Feel free to define your own constants as needed.

For Poryscript users, the Route 110 constants would look like this:
```
const LOCALID_CHALLENGE_BIKER = 21
const LOCALID_RIVAL = 28
const LOCALID_RIVAL_ON_BIKE = 29
const LOCALID_BIRCH = 36
```
It's in a different format, but it achieves the exact same effect. Just be aware that defining constants in Poryscript like this does not apply to the `raw` sections of code (the original scripting language). In that case you'd have to use `.set` to define constants (like earlier) in its own `raw` section. It's ok to use both at the same time, like so:
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

While constants are mostly used for object id numbers, they can be used for other things. For example, you can use constants to stand in for certain flags or variables you want to use in the file. The pokefirered decomp does this occasionally.  
Here is what is done in the scripts file for the Pokémon Fan Club in Vermillion City:
```
.set SPOKE_TO_WOMAN_LAST,   FLAG_TEMP_2
.set SPOKE_TO_FAT_MAN_LAST, FLAG_TEMP_3
```
That scripts file uses the two temporary flags as ways to determine whether certain NPCs have already been spoken to by the player. Using the constants helps anyone who looks at the code to better understand what those flags are used for.

Constants are never necessary, but almost always useful.

[(back to top)](#scripting)

## Map Scripts
Map scripts are special scripts that run under certain conditions; for example, after you warp into a map or after you close a menu. Every script file is required to have a mapscripts function at the top (below the constants), even if you don't use any map scripts. You cannot have more than one mapscripts function per file.  
An example of an empty mapscripts function looks like this:
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
An event script is a block of script code. If an event script is run, the entire block is run line by line, starting at the top of the event script. After the map scripts, you can add as many event scripts to the file as you want in any order. We already looked at some event scripts in the [Map Scripts](#map-scripts) section.  
Here is an example of an event script from the Rustboro City scripts:
```
RustboroCity_EventScript_LittleGirl::
	lock
	faceplayer
	msgbox RustboroCity_Text_PokemonChangeShape, MSGBOX_DEFAULT
	applymovement LOCALID_LITTLE_GIRL, Common_Movement_FaceOriginalDirection
	waitmovement 0
	release
	end
```
This is run every time you talk to a certain little girl in the southern part of the map. Let's take a look at what it does.

Every line within an event script starts with a macro. A macro is a command that tells the game what we want that line to do (more on [Macros](#macros) in a later section). The first line of our example has the macro `lock`. This tells the game to lock movement of the player, so we can't walk around while we are talking to this NPC. The next line uses the macro `faceplayer`, which makes the last object the player interacted with turn to face the player's position. Both of these macros already have all of the information they need in order to run, so it is not necessary to add anything after them on those lines. The game just runs the `lock` macro, then the `faceplayer` macro, then moves on to the next line.

The next line is:
```
msgbox RustboroCity_Text_PokemonChangeShape, MSGBOX_DEFAULT
```
`msgbox` is a macro that prints a message box on screen. This macro needs us to provide some extra information because it doesn't know what text we want to display. So on the same line after we call `msgbox`, we have to provide the specified text we want the game to print. In this case, `RustboroCity_Text_PokemonChangeShape` just refers to a text script later in the file (more on [Text](#text) in a later section). After that, we have `MSGBOX_DEFAULT`, which just tells the game what behavior we want the message box to have. 

You may notice that there is a comma`,` between the two pieces of added information, but not between `msgbox` and `RustboroCity_Text_PokemonChangeShape`. This is because the code already knows the distinction between a macro and the values that the macro asks for, so we only need a space between them. However, the code does not readily distinguish between the end of one value and the beginning of the next, so we have to clearly separate them with commas`,`.

The next two lines are:
```
applymovement LOCALID_LITTLE_GIRL, Common_Movement_FaceOriginalDirection
waitmovement 0
```
`applymovement` applies a movement to an object. It requires us to specify the object id number and the movement we want the object to make. The constant `LOCALID_LITTLE_GIRL` (as defined at the top of the file) is a stand-in for the object id `6`. `Common_Movement_FaceOriginalDirection` is the specific movement we want to apply (more on [Movements](#movements) in a later section). The next line, `waitmovement 0`, just tells the game to wait until the last called movement completely finishes before the script moves to the next line.

The macro `release` tells the game to release the `lock` we put on the player's movement at the beginning of the event script.

The last line is one of the most important components of an event script. `end` tells the game to stop running scripts. Without this, the game can get confused and potentially softlock. One of the more useful exception to this is ending an event script with `return` instead of `end`. This is useful when we call an event script from within another event script. `return` just tells the script to go back to the original event script so it can keep running. Eventually, all run scripts should run into an `end`, though. This is explained in more detail in the [Macros](#macros) section.

### Event Script Names
Every event script needs a distict name. The existing scripts already conform to a particular naming convention. Each name in this convention has three parts, each separated by an underscore`_`:
1. The map this event script is associated with.
2. The type of script it is (EventScript, Movement, Text, etc).
3. The specific identifier for that event script.

For example, let's look at the name of the event script in Rustboro City's scripts from earlier:
```
RustboroCity_EventScript_LittleGirl
```
In this name, `RustboroCity` is the map the script is associated with, `EventScript` is the type of script, and `LittleGirl` is the specific identifier. I think this is a very good naming convention to follow, as it is always clear to whoever looks at it what that script is for and where it is used. However, you can name your event scripts whatever you want.

But why would it be useful to use the associated map in an event script's name if the scripts file is already associated with a specific map? Well, that's because **any** event script in **any** file can be run from **any** other file. I can run a Rustboro City event script from the Petalburg City scripts.inc file, so it is useful to know exactly what map that script is originally associated with. This is why it is important for every event script **in every file** to have their own unique names. The code won't allow you to have two different event scripts with the same name, even if they are in different files.

### Running Event Scripts
There are a few different ways that event scripts can be run. You can use [map scripts](#map-scripts) to run them under specific conditions. Most of the time, you will be connecting event scripts to things on the map, like objects or triggers.

Take the little girl in Rustboro City from earlier as an example. To recap, her event script is named `RustboroCity_EventScript_LittleGirl`. Let's look at Porymap to see how the event script is connected to her object:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/porymap_script_example.png)

As you can see, the box labeled "Script" contains the name of the event script. It's as simple as that. Every time you interect with any object, it will run the event script that you specify in that box.

Triggers work similarly. If a trigger is active, it will run the event script in the "Script" box whenever the player steps on the trigger.

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/porymap_trigger_example.png)

Trainers who battle when they spot the player are tied to an event script like a normal object. However, in order for the battle to work properly, the macro that runs the battle must be used on the first line of the event script.  
Here's an example event script for a trainer:
```
Route110_EventScript_Jaclyn::
	trainerbattle_single TRAINER_JACLYN, Route110_Text_JaclynIntro, Route110_Text_JaclynDefeated
	msgbox Route110_Text_JaclynPostBattle, MSGBOX_AUTOCLOSE
	end
```
Once that particular trainer has been defeated, the event script will not initiate that trainer battle ever again (rematches are handled with a different macro). So if the player talks to the trainer after they are defeated, the event script will skip that first line and go immediately to the next line.

[(back to top)](#scripting)

## Movements
Movements are written in blocks of code, similarly to event scripts.  
Here's an example movement:
```
Route110_Movement_BirchApproachPlayer1:
	walk_down
	walk_left
	walk_left
	walk_down
	step_end
```
The naming convention here is the same one used for the names of event scripts. Here, the name tells us this block of code is a movement. Again, you can use whatever name you want, but the naming convention in the existing scripts is extremely useful.

Within movements, each line is a single movement action. The game applies the movement actions to the specified object one by one in the order they are listed. All of the different movement actions are defined in [asm/macros/movement.inc](https://github.com/pret/pokeemerald/blob/master/asm/macros/movement.inc). Consult this file for a complete list of available movement actions.

If you want to do multiple of the same movement action in a row, you still have to list them all out individually. In the example above, Birch takes two steps left and both steps are listed as their own lines. Sometimes, you'll see movements that have the same movement action listed 9 or 10 times in a row (as is common when an NPC walks off screen). This is not a problem with Poryscript, as you can just tell it how many times you want to run a certain movement action in a row within the same line.  
For example, the two `walk_left`s could be rewritten as `walk_left * 2` in Poryscript. See [this section](https://github.com/huderlem/poryscript?tab=readme-ov-file#movement-statement) of Poryscript's guide for more information on this.

The line `step_end` tells the game to stop applying a movement. It is necessary to put this at the end of every movement, or the game will be stuck trying to apply a movement forever. This is unnecessary with Poryscript, as it automatically adds `step_end` to the end of every movement for you.

Movements are exclusively used with the macro `applymovement` in the event scripts. `applymovement` requires two inputs from you:
1. The object id number of the object you want to apply the movement to. (See the [Constants](#constants) section for easy usage.)
2. The name of the movement you want to apply.

Unlike most other macros, `applymovement` does not wait for the movement to finish before moving to the next line. If you want to move multiple different objects simultaneously, you can just do several `applymovement` lines in a row, like so:
```
applymovement LOCALID_GRUNT_1, Route121_Movement_Grunt1Exit
applymovement LOCALID_GRUNT_2, Route121_Movement_Grunt2Exit
applymovement LOCALID_GRUNT_3, Route121_Movement_Grunt3Exit
waitmovement 0
```
These are the lines that tell the Aqua Grunts on Route 121 to walk toward Mt. Pyre. All three of them walk at the same time. That `waitmovement` macro tells the game not to run any more lines in the event script until the specified object is finished moving. If the object number is `0` (like in the example), it will wait until the most recent `applymovement` line has finished its movement.

Like event scripts, movements can be run from any file. However, it tends to be impractical to use them in files other than the one they are written in, due to the fact that they are most often very specific to individual event scripts. There is an exception to this. The file [data/scripts/movement.inc](https://github.com/pret/pokeemerald/blob/master/data/scripts/movement.inc) contains movements that are used repeatedly throughout the game. You can use these in the `applymovement` macro like you would normal movements. Feel free to make new common movements and add them to that file. It is very convenient to have all of the common movements defined in one file like that; it is easy to reference when you need it.

[(back to top)](#scripting)

## Text
Text is also written in blocks of code.
Here is an example text:
```
LittlerootTown_ProfessorBirchsLab_Text_MightBeGoodIdeaToGoSeeRival:
	.string "PROF. BIRCH: If you work at POKéMON\n"
	.string "and gain experience, I think you'll make\l"
	.string "an extremely good TRAINER.\p"
	.string "My kid, {RIVAL}, is also studying\n"
	.string "POKéMON while helping me out.\p"
	.string "{PLAYER}{KUN}, don't you think it might be\n"
	.string "a good idea to go see {RIVAL}?$"
```
The naming convention here is the same one used for the names of event scripts. Here, the name tells us this block of code is text. Again, you can use whatever name you want, but the naming convention in the existing scripts is extremely useful.

`.string` tells the code that the following text should be treated as a string of characters. This is necessary for the game to be able to print the text to a message box on screen.

Everything inside of the quotation marks`""` will be printed on screen. The only exceptions to this are the end-of-line modifiers (`\n`, `\l`, and `\p`) and the end-of-text modifier (`$`).

The end-of-line modifiers tell the game what to do at the end of each text line. Their different functions are listed here:
* `\n`: Each in-game message box has two lines that can be displayed at once. After the game has fully printed the first line, `\n` tells the game that the next string of text should be printed on the second line in-game. This should only be used if the second line of the in-game message box is empty.
* `\l`: Once both lines of text are filled in-game, `\l` can be used to tell the whole message box to scroll up to reveal the next string. This should only be used if both lines of the in-game message box are already printed to.
* `\p`: This clears out the whole message box and prints the next string to the first line of the message box.

If these modifiers are used in the middle of the text string instead of at the end, they will apply to the remainder of the current string of text. You can use multiple modifiers per string, for example:
```
.string "Whoa!\nThis is awesome.\l"
.string "Who knew you could do that?$"
```
This would appear in-game as:
```
Whoa!
This is awesome.
Who knew you could do that?
```
Keep in mind that the in-game message box is only so wide. If a string is too long it will exceed the bounds of the message box and get printed off screen. Use the end-of-line modifiers to your advantage here. The Birch text example from earlier uses end-of-line modifiers to split sentences into multiple strings to make sure everything fits inside the in-game message box properly.

There are a few ways to call special strings within your text:
* `{PLAYER}`: This is replaced with the player's name. Sometimes, you will see `{KUN}` after `{PLAYER}`. `{KUN}` is a leftover functionality from the Japanese version and is not necessary to include; it's just an empty string.
* `{RIVAL}`: This is replaced with the rival's name. In pokeemerald, it defaults to "MAY" if the player is male, and "BRENDAN" if the player is female.
* `{STR_VAR_...}`: There are three variables that can be buffered with whatever predetermined strings you want. These are STR_VAR_1, STR_VAR_2, and STR_VAR_3. See the [Macros](#macros) section for different ways to buffer these strings.

The `$` modifier just tells the game where the end of the text is. It is required in order to function properly.

Poryscript makes the text writing process so much easier. See [this section](https://github.com/huderlem/poryscript?tab=readme-ov-file#text-statement) of their guide for more information.

### Using Text in Event Scripts
Most of the time, text is going to be used with the `msgbox` macro. `msgbox` is what creates a message box on screen.  
Here's an example:
```
msgbox RustboroCity_Text_PokemonChangeShape, MSGBOX_DEFAULT
```
This line tells the game to create a message box on the screen using the specified text and the specified type of message box. `RustboroCity_Text_PokemonChangeShape` is the text that will be displayed within the message box. `MSGBOX_DEFAULT` tells the game what type of message box should be used.

The message box type is actually an optional piece of information to provide, as leaving this part of the line blank just makes the message box have the default behavior.  
Without it, the line would look like this:
```
msgbox RustboroCity_Text_PokemonChangeShape
```
Incidentally, this shortened line functions the exact same as the original line. `MSGBOX_DEFAULT` is what `msgbox` already defaults to, so there was no real need to specify it in the first place. See, even the game's original devs aren't the most concise! This isn't a wrong way to do things, just redundant.

There are several different message box types:
* `MSGBOX_DEFAULT`: The default type. It just creates a message box, prints all the text, and then waits for the player to push a button before moving on to the next line in the event script.
* `MSGBOX_NPC`: This automatically runs the `lock` and `faceplayer` macros before creating the message box. After the message box is done, it automatically runs a `release` macro. This is used when the player interacts with an NPC.
* `MSGBOX_SIGN`: This runs a `lockall` macro before the message box, and a `releaseall` macro after. This is used when a player interacts with a sign.
* `MSGBOX_YESNO`: At the end of the message box, it prompts the player to choose "YES" or "NO", then stores the choice in the VAR_RESULT variable for later use in the event script.
* `MSGBOX_AUTOCLOSE`: The same as `MSGBOX_DEFAULT`, but adds a `release` macro after the message box is done.
* `MSGBOX_GETPOINTS`: Plays the jingle for receiving Battle Points at the start of the message box. Used exclusively in the Battle Frontier when BP is rewarded. Not used in pokefirered.
* `MSGBOX_POKENAV`: This is never used.

Here are some other macros that are commonly used in conjunction with `msgbox`:
* `waitmessage`: This is often used on the line after `msgbox` to tell the game to wait until all of the text has been printed before any more event script lines can be run.
* `closemessage`: This is used on the line after `msgbox` to tell the game to close the message box before running any more lines in the event script.

[(back to top)](#scripting)

## All Together
I made some changes to the scripts for the little girl in Rustboro City from earlier. Let's use it as an example of the three main components of scripting (event scripts, movements and text) used together:
```
.set LOCALID_LITTLE_BOY, 7
.set LOCALID_LITTLE_GIRL, 8

~ ~ ~

RustboroCity_EventScript_LittleGirl::
	lock
	applymovement LOCALID_LITTLE_GIRL, RustboroCity_Movement_LittleGirlPlayGame
	applymovement LOCALID_LITTLE_BOY, RustboroCity_Movement_LittleBoyPlayGame
	waitmovement LOCALID_LITTLE_GIRL
	applymovement LOCALID_LITTLE_GIRL Common_Movement_ExclamationMark
	waitmovement 0
	applymovement LOCALID_LITTLE_GIRL Common_Movement_Delay48
	waitmovement 0
	msgbox RustboroCity_Text_PlayingAGame
	applymovement LOCALID_LITTLE_GIRL, Common_Movement_FaceOriginalDirection
	waitmovement 0
	release
	end

RustboroCity_Movement_LittleBoyPlayGame::
	lock_facing_direction
	walk_left
	unlock_facing_direction
	delay_16
	face_down
	delay_16
	delay_16
	face_left
	delay_16
	delay_16
	face_up
	delay_16
	delay_16
	delay_8
	walk_right
	step_end

RustboroCity_Movement_LittleGirlPlayGame::
	walk_left
	walk_down
	walk_left
	walk_left
	walk_up
	walk_up
	walk_right
	walk_right
	walk_down
	walk_right
	face_player
	step_end

RustboroCity_Text_PlayingAGame:
	.string "Hey, {PLAYER}!\n"
	.string "We are playing a game!\l"
	.string "You can join us if you want.\p"
	.string "Oh, wait.\nThis is only a two player game.\p"
	.string "Well, you can just watch us play from\n"
	.string "over there.$"
```
Take a look at how this plays in the game:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/gifs/scripting%20example.gif)

[(back to top)](#scripting)

## Macros

[(back to top)](#scripting)
