#Topic: The hows and beyond of Git Versioning

###The Why:
I chose this topic, because I know minimal commands and tools for git version control, so I can handle version control provided nothing goes wrong, but I have had a hard time in situations where things have gone wrong and I had to clean it up. In fact, I spent a few extra hours on one deliverable for this class because there was a big problem between the main branch on my local device and the remote branch on github. It was very frustrating and I ended up making an entirely new copy of my repository and started over, because I created more messes and my old code repository seemed unsalvageable. I want to understand git well enough to handle the messy scenarios so I don't have to redundantly copy information to start over when there's a problem. I will explore some of the other tools/operations available for better version control with git.

###My problem (for context):
For the metrics deliverable (I belive deliverable 7), after finishing my code and getting everything working, I committed and pushed my final code. There was an issue. What happened was I didn't realize that I saved a shell script in the source directory of my project, and forgot to include it in the git ignore file. This shell script included my API key, which I had previously included as a github secret. Apparently, when you try to push a change to github, it scans the commited files for any github secret, to prevent its addition to the master repository. So I got an error message warning me that my commits contained a repository secret and I couldn't push the changes to github. Then I had the problem of how to remove a commit (an already staged change) without losing my progress. I looked up some weird command, and did the command, and after a chain of other small jenky commands, the my local repository no longer was associated with the master repository in github. I don't know what I did, but it's the last time I'll go to chatGPT for git commands. I ended up cloning my repository again and then redoing the changes, because whatever I had done made it seem irreparable.

###Git commands I already knew:

- `git add` - adds a file to be associated with the repository any time I commit changes (I added the shell script command because I did `git add --all` which adds all files found in the directory (sloppy choice there))
- `git commit` - stages any changes I made to be pushed
- `git push` - sends my changes to the parent repository to update it
- `git clone` - clones a parent repository to a new location with the parent repository set so I can do git operations on the new location
- `git pull` - pulls in any changes from the remote repository to the local repository so they're in sync (this, like pushing, can get messy if the local and remote repository have both been changed)
- `git status` - lets you know the status of your repository (which files have been added or changed and if they have been committed or not)

###Git commands I would have found useful:

- `git reset HEAD~1` - This command helps for my specific situation. It undoes the latest local commit (but only if I haven't pushed the changes yet). This is what I should have done in my situation
- `git revert HEAD` - This command does the same thing, but if I have already pushed the changes
- `git rm` - This removes a file from the git project. It doesn't delete the file from my computer, but it dissociates it from the project, as if it's in the git ignore. This would have come in handy if I new that my `generatePizzaData.sh` file included in the git project. I could have executed the command `git rm generatePizzaData.sh` and then I could have commited and pushed without an issue
- `git checkout -b [branch name]` - this creates a new branch in your local environment. This can be used if I have a few changes I want to make, but they are involved with different goals and should be independent. I can make a branch called `updateMetricsForEndpoints`, then make the changes in the separate branch for that specific purpose. Then I can merge that branch into my main branch with `git merge`. It's more common to have branches created at the remote repository (at least in my work experience) for the specific task, then once that task it complete in the local environment, they can push their changes (on the unique branch) to the remote parent, the the developer creates a pull request, which solicits a review from other developers to approve its merge with the master branch. That keeps things a lot neater, and protects the parent repository (which should have solid code) from being contaminated with accidental code poison
- `git switch [branch name]` - switches between branches. But be careful. In my experimentation I've seen that git commands are sort of in sync with Visual Studio windows, so if you have two tabs open - one for your `addLogging` branch and one for your `addUnitTests` branch, both windows will likely only display the current representation of the CURRENT BRANCH. So if I do the commmand `git switch addLogging`, it will switch both windows to be making changes on that branch. If you aren't careful, you can think you're making changes to one branch while accidentally changing the other. My advice is to not work on two branches concurrently. It's too messy. Close everything up before opening the other branch.
- `git log` - shows chronologically the commit history for the repository, that way I can see exactly which commit is the issue if I need to remove a commit or something like that
- `git diff` - shows the difference between the local repository and the latest version of the remote repository. This is useful because I can see side by side how far behind I am, or what I need to change between my local and remote repositories
  These commands and their simple information I pulled from the git docs on the git website: https://git-scm.com/docs

###Random fun:
Random side note. In looking into extra git commands and using them to help me more carefully manage version control, I discovered that there is a git book, which is a comprehensive guide to git. Book found here: https://git-scm.com/book/en/v2. A lot of times I learn better when reading generally without a need, rather than searching specifically for one piece of information, because reading generally, when I'm invested, lets me explore less specific avenues and discover something I wouldn't have found out otherwise, then to have that information in my toolbox of useful knowledge to use later. The git book is something I want to read through, not looking for anything, just to understand more of the basic principles of good version control. I've read a few tech books, and maybe this can be my next one.

###VS Code Source Control
I actually have never once (as far as I recall) done any git source control through VS code. I have always done it through my git bash terminal, because

1. It was the first thing I learned and I didn't want to try a new thing if what I was doing already workd
2. I thought there was a cooler stigma around doing terminal commands instead of using the VS code tools.

Apparently, there's an entire `Source Control` view in VS Code dedicated to git source control and it's pretty awesome. One benefit of this over the terminal is how many more visual aids there are, that probably would have helped me avoid the big mistake I mentioned above.

It's pretty cool to navigate the Source Control view and see the different options, like a Graph that shows all the most recent commits, which I can click on to see all changed in previous commits an restore any that I need to, or even just to see old code for a reference. I can also see a side by side of the files I've changed in any commit I want to push, so there is no speculation about what I've chaged. I think the user friendly interface and commands have made it really easy to visualize the changes, and I wish I had this familiarity with the VS code tools earlier. It probably would have saved me from a world of hurt. Even just committing and pushing my last changes in VS Code has made me more familiar with it.

I also found out that in the control Palette (get there by typing `ctrl+shift+P`), I can simply look up the control `Git: Undo Last Commit` to undo the last commit I had staged. As cool as it is, it's kind of annoying to know that the solution to my problem was this easy. But that's okay, now I know.

All of the information from this section was gathered from the page on the Visual Studio Code website called `Using Git Source Control in VS Code`
