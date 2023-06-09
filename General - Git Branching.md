#git #github #devops #skills #general

Building software is a functionally endless process made up of a series of small milestones. Each milestone is really just adding or rewriting code across multiple files in the project. Git has the ability to keep track of all the changes made to a codebase, and importantly, can do so across multiple branches that can then be merged at a later date.

## Why branch?
There are two main use cases for branching, team collaboration and development of a product that has users. Branching allows multiple people to work on the same files at the same time. It also allows changes to be made to a version of the software that is not in the hands of users or on a production server, and it can be merged into the production branch after proper testing and review without interrupting service. 

When working on personal projects alone, you often don't have any need to work across multiple branches. When not using branches, all work is being done on the main branch, the trunk of the tree. However if the project is hosted and you want to develop new features without interrupting service, you should create a branch to do this work on.

## Working with branches
You can see all the branches in a project with the command `git branch`. To switch between branches you can use `git checkout <branch name>`, and you can create a branch and immediately switch to it by adding the -b flag to this command. 

When you commit changes to your new branch, they will only be on that branch, the main branch remains in whatever state it was in before. To push your branch to the remote for the first time after making it locally use `git push -u origin <branch name>`

When you are done working on a branch, you will want to merge the changes made there into the main branch again. You do this by switching back to the main branch with checkout, and then running `git merge <feature branch name>`.

If someone else was working on the main branch while you were on your feature branch, and you both changed the same piece of code, you will get a merge conflict. You have the optiion to accept one or both of the changes to complete the merge, but it is often best to simply abort the merge operation and manually change the files to agree before merging again. This is a good illustration of why making many small commits is generally a good rule to follow.

You should also keep in mind that if you have other people contributing to the project, you should regularly merge the master into your feature branch to ensure that you have the latest version of the code. This will reduce the number and severity of merge conflicts when you are ready to merger your feature into the master.

If you have made many commits to the feature branch, but don't want all of them in the change history of the main branch, you can use `git merge <feature branch> --squash`. This will squish all feature branch commits into one and then merge them with the main branch. You then need to add a commit to the main branch with a message reflecting that you merged, because merging with the squash flag does not update the head commit.