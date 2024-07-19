# Commonly Used Files
*Contributors: Bivurnum*

This is a list of files in the decomps that are frequently edited by ROM hackers. Each one will have a brief explaination of how that file is used. All of them link to the file in pokeemerald, but pokefirered has the same file unless otherwise specified.

### Scripting
* [data/maps](https://github.com/pret/pokeemerald/tree/master/data/maps): Within this folder, open the folder of the map whose scripts you want to edit. The scripts are all contained in the scripts.inc file (or scripts.pory for Poryscript). pokefirered separates its text from the scripts into their own text.inc files.
* [include/constants/map_scripts.h](https://github.com/pret/pokeemerald/blob/master/include/constants/map_scripts.h): Not commonly edited at all, but a good file to reference how the different map scripts function. (pokefirered's file does not contain the same information, but the information in pokeemerald's file is equally useful for pokefirered.)
* [asm/macros/event.inc](https://github.com/pret/pokeemerald/blob/master/asm/macros/event.inc): This file contains most of the macros used for scripting. You can write your own macros here for use in the scripts files.

### Flags and Variables
* [include/constants/flags.h](https://github.com/pret/pokeemerald/blob/master/include/constants/flags.h): This is where all of the game's flags are defined. Flags that are unused in the base game will be labeled as such (this is less clear in pokefirered).
* [include/constants/vars.h](https://github.com/pret/pokeemerald/blob/master/include/constants/vars.h): This is where all of the game's variables are defined. Variables that are unused in the base game will be labeled as such (this is less clear in pokefirered).

### Trainers
* [include/constants/opponents.h](https://github.com/pret/pokeemerald/blob/master/include/constants/opponents.h): This is where all of the game's trainers are defined. A trainer MUST be defined here in order to make them able to battle (the exception to this is the Battle Frontier trainers).
* [src/data/trainers.h](https://github.com/pret/pokeemerald/blob/master/src/data/trainers.h): This is where all of the trainers' data is determined except for their Pokémon teams.
* [src/data/trainer_parties.h](https://github.com/pret/pokeemerald/blob/master/src/data/trainer_parties.h): This is where the trainers' Pokémon teams are determined.

### Strings
* [src/strings.c](https://github.com/pret/pokeemerald/blob/master/src/strings.c): This is where the game's predetermined strings are defined.
* [src/data/script_menu.h](https://github.com/pret/pokeemerald/blob/master/src/data/script_menu.h): This is where multichoice lists are defined. In pokefirered, this is located in [src/script_menu.c](https://github.com/pret/pokefirered/blob/master/src/script_menu.c).

### New Game
* [data/scripts/new_game.inc](https://github.com/pret/pokeemerald/blob/master/data/scripts/new_game.inc): This is where the game resets all of the berry trees and sets flags that should be set before a new game starts (these are mostly to hide objects that shouldn't appear until triggered later). pokefirered doesn't have this file.
* [src/main_menu.c](https://github.com/pret/pokeemerald/blob/master/src/main_menu.c): This is where the main menu is handled (where you choose to continue or start a new game). This is also where Birch's intro speech is handled. In pokefirered, Oak's intro speech is handled in [src/oak_speech.c](https://github.com/pret/pokefirered/blob/master/src/oak_speech.c).
* [src/starter_choose.c](https://github.com/pret/pokeemerald/blob/master/src/starter_choose.c): This is what handles the screen where the player chooses the starter Pokémon from Birch's bag. Obviously, this file is not in pokefirered, which handles its starter choice in the scripts.

# **pokeemerald-expansion**
These files are exclusive to pokeemerald-expansion.

### Config
These are all files where you can toggle different features and mechanics to your own custom preferences. The default for most mechanics is GEN_LATEST, so you only really have to change things if you want to revert certain mechanics to how they were in a previous generation. All of the other toggles are either TRUE/FALSE or are sufficiently explained within the file.
* [include/config/battle.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/include/config/battle.h)
* [include/config/debug.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/include/config/debug.h)
* [include/config/item.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/include/config/item.h)
* [include/config/level_caps.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/include/config/level_caps.h)
* [include/config/overworld.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/include/config/overworld.h)
* [include/config/pokemon.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/include/config/pokemon.h)
* [include/config/save.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/include/config/save.h)
* [include/config/species_enabled.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/include/config/species_enabled.h)

## Partner Trainers
* [include/constants/battle_partner.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/include/constants/battle_partner.h): This is where all of the partner trainers are defined (for example, the multi battle in the Space Center with Steven on your side). A partner MUST be defined here in order to make them able to battle alongside the player.
* [src/data/battle_partners.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/src/data/battle_partners.h): This is where all of the partner trainers' data is determined except for their Pokémon teams.
* [src/data/partner_parties.h](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/src/data/partner_parties.h): This is where partner trainers' Pokémon teams are determined.
