# Scripting

## Contents
* [Intro](#intro)
* [Constants](#constants)
* [Mapscripts](#mapscripts)

## Intro
Scripts are how we tell the game how to handle most things that happen in the overworld. Each map section has its own file that defines all of the scripts that happen within that area. To find the scripts, navigate to data/maps. Then open the folder of the map you are trying to change the scripts for. The scripts will all be located within the scripts.inc file, which is where we will be doing our editing. (If you are using Poryscript, you will be editing scripts.pory instead.)

## Constants
Let's start at the top of the scripts file. Not every map will have them, but if any constants are defined they will be at the top of the file. Every object event (NPCs, Cut trees, Briney's boat, etc.) is designated an object id number on the map they are in. If you click on an object event in Porymap, it will display its corresponding object id, like so:

![](porymap_selected_object.png)

This is the map for Route 110 in Emerald. As you can see, Prof. Birch's object id is 36. The decomp only knows him by this number, so if we want to tell the game to do something with him (like make him walk to the player) we will need to use that number when calling his actions in the scripts.

But 36 is a lot of objects to keep track of on just one map! How is anyone expected to remember which object id number corresponds to which object? It would be tedious to have to switch back and forth between the scripts and Porymap all the time just to check for the number you need.

Well, not to worry! This is where the constants come in. Defining a constant at the top of the page allows you to connect a more user-friendly name to each of the object id numbers you'd like to use in the scripts. Here is an example from the Route 110 scripts:  
```
.set LOCALID_CHALLENGE_BIKER, 21
.set LOCALID_RIVAL, 28
.set LOCALID_RIVAL_ON_BIKE, 29
.set LOCALID_BIRCH, 36
```
The `.set` here tells the decomp that we want to define a new constant. The `LOCALID_...` is the name of the constant you want to define. And the number is the object id you want to assign to the constant. Now you can use the constant in place of the object id number within this script file. For example, this macro:
```
applymovement LOCALID_BIRCH, Route110_Movement_BirchEntrance
```
That is used much later in the scripts file to tell the Prof. Birch object to walk on screen. Using the constant `LOCALID_BIRCH` allows anyone who looks at the code (including you) to know exactly which object you are applying the movement to. The decomp still reads it as 36, so nothing has fundamentally changed about the code except our ability to read it easier. This is an extremely convenient functionality to take advantage of. Feel free to define your own as needed.

For Poryscript users, the Route 110 constants would look like this:


## Mapscripts
