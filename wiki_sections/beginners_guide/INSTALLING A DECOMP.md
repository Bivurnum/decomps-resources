## Installing a Decomp
*Contributors: Bivurnum and RavePossum*

Once you've chosen which decomp you want to work with, follow the instructions listed in the Install.md file within that decomp's repository on GitHub. Here are the links to those instructions:
* [Install pokeemerald](https://github.com/pret/pokeemerald/blob/master/INSTALL.md)
* [Install pokeemerald-expansion](https://github.com/rh-hideout/pokeemerald-expansion/blob/master/INSTALL.md)
* [Install pokefirered](https://github.com/pret/pokefirered/blob/master/INSTALL.md)

The process of installing a decomp can be a bit lengthy, but once it is set up correctly you shouldn't ever have to do it again. The only exception to this is pokeemerald-expansion, which has a different installation than the others, so if you are going to work with both pokeemerald and pokeemerald-expansion you'll have to follow the installation instructions for both.

Notes:
* If any of the commands don't work properly, try closing the console and then opening it again as an administrator. Then run the commands again.
* If you're having difficulty implementing the instructions in the INSTALL.md file, you could try following [this guide](https://www.pokecommunity.com/threads/tutorial-how-to-build-the-pok%C3%A9mon-gba-decomps-using-wsl-win10.432351/) (by Lunos) instead (this may not work for pokeemerald-expansion).

RavePossum's advice if you're having trouble accessing the proper directory with the `cd` command:
> If you're having trouble changing to a full file path directory with `cd` I'd recommend trying it step by step. First, `cd /mnt/c` to get to your C drive. Then from there you can `cd users`, then `cd` to your user name,  `cd documents`, and `cd decomps`. If any of those say it doesn't exist, you can run a `ls -l` to see all of the contents of the current folder, where you should be able to see the name of whatever folder you want to `cd` to if it does indeed exist

Most of the install instructions are just setting up the compiler so you can compile a new ROM with your code changes. Once you get to the section labeled "Installation", you are all set up to compile the ROM. From here, you just need to get the repository with all of the code files onto your device. Following the instructions in the "Installation" section will allow you to do that, but you may find it easier at this point to follow the instructions in the [GitHub Desktop](https://github.com/Bivurnum/decomps-resources/wiki/GitHub-Desktop) section of this guide instead.
