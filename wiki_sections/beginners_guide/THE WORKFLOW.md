## The Workflow
*Contributors: Bivurnum*

These are generally the steps you will take every time you wish to work on your decomp project:
1. Open GitHub Desktop. Make sure the selected branch is the project you want to work on.
2. Open the console you use to compile the ROM. (Ubuntu, Debian, MSYS2, whatever you set up in the install instructions, where you run your "make" command)
3. Enter the command that changes the directory to the project folder you're working with. This will be something like  
`cd /mnt/c/Users/<user>/Desktop/decomps/pokeemerald-expansion`. (The "*<user*>" here would be your username on your computer.) I personally have this command written in the Notepad app so I can easily copy/paste it into the console.
4. Open the editing tools you'd like to use. (Visual Studio Code, Porymap, etc.)

Once that is done, you're ready to work on your ROM hack. This next list of steps is generally what you'll follow to make changes and test them. Rinse and repeat as necessary:
1. Make the changes you want, either in the code directly or with the help of a program like Porymap.
2. Save all of your changes! I can't tell you the number of times I've gone to test my changes, only to find that they weren't applied because I forgot to save.
3. Run a `make` or `make -j` command in the console. Wait for the process to end.
4. If you get any errors, make changes to fix them and then repeat steps 2 through 4. Otherwise (the last line should end with "silent") continue to step 5.
5. Use an emulator to open the ROM found in your project's folder. It will have the same name as the decomp you're working with. (For example, if you're working with pokeemerald the ROM will be called "pokeemerald.gba".)
6. Test your changes in the game. If they don't work the way you want, repeat steps 1 through 6. Otherwise continue to step 7.
7. Once you have your changes how you want them, go to GitHub Desktop and make a commit detailing your changes. I also advise you immediately push that commit to the online repository.

When you are finished working on your project for now, you can just close everything normally. When you want to come back to it, just follow the instructions at the beginning of this section again.
