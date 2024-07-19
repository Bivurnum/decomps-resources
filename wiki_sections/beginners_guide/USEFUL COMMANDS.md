## Useful Commands
*Contributors: Bivurnum and RavePossum*

This is a list of commands that can be useful when working with a decomp. These are all used in the console (Ubuntu, Debian, MSYS2, whatever you set up in the install instructions).

* `make` is used to compile a new ROM from the project files, including any changes you made. This will replace any existing ROM with the same name within your project's folder.

* `make -j` is a version of `make` that utilizes parallel building. This usually makes the ROM compile faster.

* `git remote add <name> <repo>` is used to add a repository as a remote. This is a useful prerequisite for the `git pull` command. Be sure to replace `<name>` with the name you want to associate with the remote; I advise using the name of the owner of the repo. Replace `<repo>` with the web address of the repository you're adding.  
(An example of this in action is `git remote add Bivurnum https://github.com/Bivurnum/pokeemerald`.)

* `git pull <remote> <branch>` is used to pull changes from another branch into your project. Be sure to replace `<remote>` with the `<name>` you defined in `git remote add`. Replace `<branch>` with the exact name of the branch you are trying to pull. There is more information on this process in the [Pulling Feature Branches](https://github.com/Bivurnum/decomps-resources/wiki/Pulling-Feature-Branches) section.  
(An example of this in action is `git pull Bivurnum all-npcs-walk`.)

* `make clean` is used to remove old build artifacts and force the next `make` to reconvert all of the files that get converted to a format that the compiler can read. This conversion doesn't happen every time you compile the ROM (in order to make compilation faster because it doesn't need to convert everything every time). Keep in mind that this does not compile the ROM.

* `make mostlyclean` does exactly what `make clean` does, but without affecting things in the "tools" file. You most often won't need to do any changes to the tools (Poryscript being a notable exception), so `make clean` is most often less necessary than `make mostlyclean`. You should run this command if you make big changes to the graphics (like ghoulslash's [Overworld Expansion](https://github.com/ghoulslash/pokeemerald/tree/overworld-expansion)). Keep in mind that this does not compile the ROM.

<!-- Add git grep -->

* `^C` is not so much a command as it is a useful prompt. Pressing the CONTROL key and the C key together while the console is running a command will stop the operation. This is useful if you need to interrupt the compilation process.

RavePossum's helpful hint:
> So let's say for example there's code you need to replace called ExampleFunction, but it's not there anymore.  
If you run `git log -n 1 -S ExampleFunction`, git will present you with the last commit the function was present in. And looking up that commit in a git viewer or on github should give you an idea of what it was replaced with.  
`-n 1` means pull the first result.  
`-S ExampleFunction` means find the string "ExampleFunction" in code.
