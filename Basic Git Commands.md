#git #github #devops

### Getting a new or existing project from your computer to github
The first thing to do is create a git hub repo from your browser, this is going to be the [[Remote Branch]] of the git repo. Copy the https URL for the repo to clipboard for later use.

Next run these commands from a terminal at the root of the project you wish to put up on github:

1. `git init`
2. `git add .` This will stage all the files in the repo
3. ` git commit -m "Initial commit"`
4. `git remote add origin <the https url for the github repo>`
5. `git push -u -f origin main` 

Make sure that your default branch name is set to main, or else the last command will need a different argument in that place. Main is the current github default but it used to be master. You can change your local gits default branch name with: 
					`git config --global init.defaultBranch <name>`
Previously I had been using `git push <url>` every time I pushed updates to a project to git hub, but adding the url to the remote origin so that every future push can be `git push origin <branch>` is much better.

### Get a file that is on the remote branch but not the local branch
The command to do this is `git pull <https add>`. This will also merge conflicts between the branches in favor of the remote branch.

