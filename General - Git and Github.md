#git #github #devops #skills 

Git is a system for tracking changes in documents, across multiple versions and with multiple people being able to edit the documents on different machines.

## Git Terminology

The basics section will cover setup and commands for the most common operations, before we dive deeper into the concepts and more difficult operations.

## Basic Git commands

### Getting a new or existing project from your computer to github
The first thing to do is create a git hub repo from your browser, this is going to be the remote branch of the git repo. Copy the https URL for the repo to clipboard for later use.

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

## A simple professional git workflow
The following set of steps are adapted from [this resource](https://github.com/firstcontributions/first-contributions) and is an attempt at emulate the standard workflow with git that you will need to know in a proffesional environment, or when contributing to open source projects.

### Forking a repo
The first step to contributing to a repository, be it an open source project or something you have been assigned to work on at your job, is making your own fork of the repo. A fork is **a new repository that shares code and visibility settings with the original “upstream” repository**. Forks are often used to iterate on ideas or changes before they are proposed back to the upstream repository.

Fork a repository by clicking this button:![](https://camo.githubusercontent.com/fcf9a4ed664cc63de2fcb14d1135072ba6d4c74a8e9bdb224ad6ab1e72600c3b/68747470733a2f2f6669727374636f6e747269627574696f6e732e6769746875622e696f2f6173736574732f526561646d652f666f726b2e706e67)
This will create a copy of the repo on your own github account.

### Clone the repository
Now you have a copy of the project on your github account online, but not on your local machine where you can easily work on it. To achieve this you need to clone it. Open the repo on github and click the code button: 

![](https://camo.githubusercontent.com/4c3f7f1bec4f04db40ecf58dc2e19c2d8992f100f3bbbc4767a9d20b29f4a43d/68747470733a2f2f6669727374636f6e747269627574696f6e732e6769746875622e696f2f6173736574732f526561646d652f636c6f6e652e706e67)
Then copy the https address for the repo to your clipboard. Open a terminal and navigate to a folder where you want the cloned repo to be. Then enter `git clone <repo-url>` (ommit the braces).

### Make a new branch
Now create a branch using the `git switch` command:
```
git switch -c your-new-branch-name
```
For example:
```
git switch -c add-shaun-ferris
```
The git switch command is used to change branches, and the -c flag tells it to create a new branch and then immediately switch onto it.

### Work on your contribution and push
Now that you have forked the repo, cloned it to your machine and created a new branch for your own work, you can get to work. Once you have made changes, stage them with `git add <filename>` or `git add .` to add all changed files. Then commit and push the changes with `git commit -m "<commit-message>" && git push origin <your-branch-name>`

If it is your first push to the remote you should use `git push -u origin <your-branch-name>`.
The `-u` flag sets the upstream branch for the current branch.When you push changes to a branch for the first time using the `git push` command, Git will not know which branch to push the changes to on the remote repository. In this case, you can use the `-u` flag to set the upstream branch for the current branch.

The syntax for using the `-u` flag is:
`git push -u <remote-name> <branch-name>`
After using this command, subsequent `git push` commands will use the specified remote and branch as the default upstream branch for the current branch. This means that you can simply run `git push` without any additional arguments to push changes to the upstream branch.

Additionally, when you run `git status`, Git will display the tracking information for the current branch, including the upstream branch that has been set with the `-u` flag.

### 