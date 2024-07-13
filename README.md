<div align="center">

# How to Use Decomps to Hack Pokémon GBA Games

</div>

## Contents
* [Introduction](#introduction)
* [Games with Decomps](#games-with-decomps)

## Introduction
You've decided you want to hack a Pokémon GBA ROM. Great! Welcome to the world of gen 3 ROM hacking!

There are two different ways to hack: Binary and Decomps.

***Binary***: Binary hacking is what most people think of for hacking Pokémon. This is the traditional way of doing things. You take a ROM and use a variety of programs to directly edit that ROM's code. While still useful, binary hacking will not be covered in this guide. We will be using something completely different.

***Decomps***: The code within a GBA ROM is generally unreadable to most humans. However, the code for both the Emerald and FireRed versions of Pokémon have been decompiled into a programming language that is much easier for people to work with: the C programmong language. ("Decomp" is short for "decompilation.") This allows us to make changes to the code more directly and in more specific ways. Because it is a decomp, we are not working with a ROM directly; we make the changes we want to the code and then compile a brand new ROM that includes our changes. It is thanks to [pret](https://github.com/pret) that we have these decomps to work with.

[Here is an overview](https://github.com/pret/pokeemerald/wiki/Why-should-I-use-this-over-binary-hacking) of the differences between the two hacking methods and the advantages of using a decomp over binary hacking.

## Games with Decomps
* [pokeemerald](https://github.com/pret/pokeemerald/blob/master/INSTALL.md): The decomp of Pokémon Emerald. This is the one with the most documentation, tutorials, and support. There is also [pokeemerald-expansion](#pokeemerald-expansion), which is a highly enhanced version of pokeemerald. The expansion will be covered in more detail in its own section below.
* [pokefirered](https://github.com/pret/pokefirered/blob/master/INSTALL.md): The decomp of Pokémon FireRed. This is the one to chose when you specifically want a gen 1 remake as your ROM base. While not as extensively supported as pokeemerald, pokefirered functions similarly to pokeemerald and many of the tutorials can be applied to both.

While pokeruby exists, it has minimal support and Ruby is so similar to Emerald in structure that you are better off using pokeemerald instead.

I won't be covering it in this guide, but it is worth mentioning that the Pokémon Pinball game for the GBA also has a decomp:
* [pokepinballrs](https://github.com/pret/pokepinballrs): Pokémon Pinball: Ruby & Sapphire
