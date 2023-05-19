#projects 

This is my first attempt at a full-stack app that I will actually deploy and use in real life.

## Purpose and goals
The site should verify user identity using an auth service, and only allow sign in for whitelisted credentials. This was the site can be hosted live on the open internet, but still only be accessible to memebers of the household.

The site should have multiple pages that cover home organization and communication of shared tasks. Key features to implement are:
- Todo list. This should feature a column for completed tasks and one for pending tasks, with the ability to mark a task as completed to move it, and the ability to clear the whole list. Tasks should be marked with the user that added them, and the user that marked them as complete. DB integration will give this component persistance.
- Budget Tracker. This page should allow for tracking of expenses against a given budget amount. Basically an implementation of the UI built in [[Project Notes - Budget App]].
- Shopping list. This page should track items added, which user added them and when. It should allow the items to be marked as collected and allow for the list to be cleared.

## Technologies
- Next.js with React.js
- MongoDB for database and Prisma for ORM
- Tailwind CSS for styling
- NextAuth for user authentication

## Process
First steps will be the same for basically any Next.js project:
1. Make a git repo - keep it private for now
2. Scaffold the next repo with `npx create-next-app@latest`
3. Git init the repo, connect to the remote, commit the contents and push the initial commits
4. Install dependancies