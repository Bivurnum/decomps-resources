<div align="center">

# How to Use Decomps to Hack Pokémon GBA Games

</div>

## Contents
* [Introduction](#introduction)
* [Games with Decomps](#games-with-decomps)
* [pokeemerald-expansion](#pokeemerald-expansion)
* [Installing a Decomp](#installing-a-decomp)
* [GitHub Desktop](#github-desktop)

## Introduction
You've decided you want to hack a Pokémon GBA ROM. Great! Welcome to the world of gen 3 ROM hacking!

There are two different ways to hack: Binary and Decomps.

***Binary***: Binary hacking is what most people think of for hacking Pokémon. This is the traditional way of doing things. You take a ROM and use a variety of programs to directly edit that ROM's code. While still useful, binary hacking will not be covered in this guide. We will be using something completely different.

***Decomps***: The code within a GBA ROM is generally unreadable to most humans. However, the code for both the Emerald and FireRed versions of Pokémon have been decompiled into a programming language that is much easier for people to work with: the C programmong language. ("Decomp" is short for "decompilation.") This allows us to make changes to the code more directly and in more specific ways. Because it is a decomp, we are not working with a ROM directly; we make the changes we want to the code and then compile a brand new ROM that includes our changes. It is thanks to [pret](https://github.com/pret) that we have these decomps to work with.

[Here is an overview](https://github.com/pret/pokeemerald/wiki/Why-should-I-use-this-over-binary-hacking) of the differences between the two hacking methods and the advantages of using a decomp over binary hacking (by LOuroboros).

This guide is mostly informed by information I have stumbled into over time, as well as my own personal experiences. If anyone has any suggestions for additions or changes to this guide, please send me a message [on pokecommunity](https://www.pokecommunity.com/conversations/add?to=Bivurnum) or [on Reddit](https://www.reddit.com/user/Bivurnum/).

## Games with Decomps
* [pokeemerald](https://github.com/pret/pokeemerald/blob/master/INSTALL.md): The decomp of Pokémon Emerald. This is the one with the most documentation, tutorials, and support. There is also [pokeemerald-expansion](#pokeemerald-expansion), which is a highly enhanced version of pokeemerald. The expansion will be covered in more detail in its own section below.
* [pokefirered](https://github.com/pret/pokefirered/blob/master/INSTALL.md): The decomp of Pokémon FireRed. This is the one to chose when you specifically want a gen 1 remake as your ROM base. While not as extensively supported as pokeemerald, pokefirered functions similarly to pokeemerald and many of the tutorials can be applied to both.

While pokeruby exists, it has minimal support and Ruby is so similar to Emerald in structure that you are better off using pokeemerald instead.
There are no decomps for LeafGreen or Sapphire because they are so similar to their counterpart games.

They are not decomps, but the gen 1 and gen 2 games (GB & GBC) have disassemblies that work similarly to decomps. The main difference here is that they are written in their own assembly languages, not C. As such, they have their own sets of tools and tutorials separate from decomps. Consult their respective repository wikis for more information:
* [pokered](https://github.com/pret/pokered/wiki/Tutorials): Pokémon Red
* [pokecrystal](https://github.com/pret/pokecrystal/wiki): Pokémon Crystal

I won't be covering them in this guide, but it is worth mentioning that these Pokémon spinoff games for the GBA also have decomps:
* [pokepinballrs](https://github.com/pret/pokepinballrs): Pokémon Pinball: Ruby & Sapphire
* [pmd-red](https://github.com/pret/pmd-red/blob/master/INSTALL.md): Pokémon Mystery Dungeon: Red Rescue Team

## pokeemerald-expansion
The amazing team with ROM Hacking Hideout (RHH) have spent years putting together a version of pokeemerald that includes modern Pokémon mechanics and a plethora of quality of life features. The amount of features they've added is staggeringly large, but here is a general list of features that make pokeemerald-expansion great:
* Physical/Special Split: Games from gen 4 onward had the individual Pokémon moves designated as doing either physical or special damage (ie, Waterfall is physical while Surf is special), rather than the gen 3 moves whose physical/special designation was tied only to the move's type. This brings the game up to the more modern standard.
* Modern Battle Engine: Battle mechanics change with each new generation of Pokémon games. Most game's battle mechanics are included in pokeemerald-expansion and can be individually toggled to different generation standards. For example, you could set the move Toxic to have the gen 4 mechanic where the accuracy is unaffected by the attacking Pokémon's type, while aslo setting the gen 6 mechanic where Pokémon gain experience when you capture a wild Pokémon. It is highly customizable to your needs.
* All Pokémon Species: As of this writing, the expansion includes all official Pokémon species from the main games through the gen 9 DLCs.
* Mega Evolution, Z-moves, and Dynamax: All of these features are implemented with the same mechanics they had in their respective releases. Terastalizing is not yet fully implemented.
* All Items: All of the items from the various mainline games are included in the expansion.

While pokeemerald-expansion is based on pokeemerald, it is not meant to be just an updated version of Emerald. In fact, several of the expansion's developers have stated that they don't think the game is beatable in its current state. The intent behind the expansion is to give hackers a base to work with that has as many options already baked into it as possible. It isn't its own ROM hack, it is a sophisticated tool for people to use to create their own ROM hacks. If you are looking to include modern mechanics in your ROM hack, I highly suggest you start with pokeemerald-expansion.

You can find the [install instructions for pokeemerald-expansion here](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/INSTALL.md).

## Installing a Decomp
Once you've chosen which decomp you want to work with, follow the instructions listed in the Install.md file within that decomp's repository on GitHub. The links in the previous sections take you to the respective install instructions. The process of installing a decomp can be a bit lengthy, but once it is set up correctly you shouldn't ever have to do it again. The only exception to this is pokeemerald-expansion, which has a completely different installation than the others, so if you are going to work with both pokeemerald and pokeemerald-expansion you'll have to follow the installation instructions for both.

Note: If any of the commands don't work properly, try closing the console and then opening it again as an administrator. Then run the commands again.

Most of the install instructions are just setting up the compiler so you can compile a new ROM with your code changes. Once you get to the section labeled "Installation", you are all set up to compile the ROM. From here, you just need to get the repository with all of the code files onto your device. Following the instructions in the "Installation" section will allow you to do that, but you may find it easier at this point to follow the instructions in the [GitHub Desktop](#github-desktop) section of this guide instead.

## GitHub Desktop
I hacked with decomps for over a year without knowing the existence of GitHub Desktop. I'm not exaggerating when I say this would have saved me HUNDREDS of hours of work and headaches had I been using it.

The benefits of using GitHub Desktop:
* Your project is easliy backed up to its own online repository, so you always have access to it no matter what happens.
* You can easily create commits, which act as sort of checkpoints for your project. If you ever need to revert back to an earlier version of your project, you can just tell it which commit to revert back to instead of reverting your changes manually.
* Switching between multiple projects is simple and straightforward.
* It has compatibility with code editors to help track changes and easily resolve merge conflicts.

I cannot overstate how much easier GitHub Desktop makes working with decomps. Follow [the guide here](https://github.com/Pawkkie/Team-Aquas-Asset-Repo/wiki/The-Basics-of-GitHub) (by Pawkkie and RavePossum) to set it up specifically for the gen 3 decomps. It's not necessary to use GitHub Desktop to hack with a decomp, but why wouldn't you use something that will make your life that much easier?
