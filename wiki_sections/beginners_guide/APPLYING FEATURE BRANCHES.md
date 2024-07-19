## Applying Feature Branches
*Contributors: Bivurnum and RavePossum*

Some hackers develop features in their own branches that other people might find useful to include in their projects. These are called feature branches, and they are generally quite easy to implement.

### Manual Implementation
When first starting out, it is recommended that you enter your desired feature branch into your project manually. Changing the code yourself is a good way to get familiar with how it works. It can also make you aware of conflicts between the feature branch and your project, so you can fix them right then and there instead of having to look for problems later.

There is a convenient way to look up all of the changes that need to be done and which files need to be altered. If you go to the feature branch's repository page on github.com, near the top of the page, click where it says how many commits ahead of the upstream it is:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/github_commits_ahead.png)

This will bring you to a page like [this one](https://github.com/pret/pokeemerald/compare/master...ghoulslash:pokeemerald:overworld-expansion). If the branch has a lot of commits, you may have to click where it says the number of files changed. From here, you just have to scroll down and make the changes that are listed. Things in green are to be added to the file, while things in red need to be removed. Just be patient and try to learn from what code is being replaced or added. Your skill will increase with repeated practice.

A good place to ask for help with these sorts of things is the [decomps forum](https://www.pokecommunity.com/forums/decomps-disassemblies.436/) at pokecommunity. Just be sure to be as specific with your problem or question as possible. And always be polite and thank those who take the time to assist you.

### Pulling a Branch
Once you are more comfortable working with the code, you can try pulling a feature branch directly into your project.  
I'll go through an example to help illustrate how this is done:

I have personally developed a feature branch that adds walking animations to Emerald NPCs that don't originally have any. If you wanted to merge that feature into your own project, you would enter these commands into your console:

`git remote add Bivurnum https://github.com/Bivurnum/pokeemerald`  
`git pull Bivurnum all-npcs-walk`

The first command just tells git which repository you want to pull from. The second command pulls the changes from my branch titled "all-npcs-walk" and tries to apply them to your project. It may take the console some time to process things after you enter the `git pull` command, so be patient. It will likely prompt you to enter a merge commit message similar to this:

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/merge_commit_message.png)
![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/merge_commit_actions.png)

If it prompts you to enter a commit message, you can just press the CONTROL key and the X key at the same time (to Exit) and it will continue merging the branch into your project.

If you get any merge conflicts, the console will give you a line like this:  
`Automatic merge failed; fix conflicts and then commit the result.`  
You'll have to resolve the conflicts before the branch can be implemented into your project. This is where VS Code comes in handy. If you open your project in VS Code after trying to merge, it will show you where all of the conflicts occur in the code (click the third icon along the left side of the window):

![](https://github.com/Bivurnum/decomps-resources/blob/main/assets/images/VS_CODE_merge_conflicts.png)

The files listed under "Merge Changes" are the ones with conflicts. Click on a file, then click on the "Resolve in Merge Editor" button in the lower right of the window. This will bring up a screen where you can choose how you want the incoming change to be applied. Once you have it how you want it, click on the "Complete Merge" button in the lower right of the window. Do this process for all of the files with conflicts. There may be multiple conflicts per file.

Once you have all of the conflicts resolved, you can click the button that says "√ Commit" (or make a commit through GitHub Desktop) and it will fully merge the branch into your project.
