 shadow-[ins#projects 

This is my first attempt at a full-stack app that I will actually deploy and use in real life.

## Purpose and goals
The site should verify user identity using an auth service, and only allow sign in for whitelisted credentials. This was the site can be hosted live on the open internet, but still only be accessible to members of the household.

The site should have multiple pages that cover home organization and communication of shared tasks. Key features to implement are:
- Todo list. This should feature a column for completed tasks and one for pending tasks, with the ability to mark a task as completed to move it, and the ability to clear the whole list. Tasks should be marked with the user that added them, and the user that marked them as complete. DB integration will give this component persistence.
- Budget Tracker. This page should allow for tracking of expenses against a given budget amount. Basically an implementation of the UI built in [[Project Notes - Budget App]].
- Shopping list. This page should track items added, which user added them and when. It should allow the items to be marked as collected and allow for the list to be cleared.

## Technologies
- Next.js with React.js
- MongoDB for database and Mongoose for ODM management
- Tailwind CSS for styling
- NextAuth for user authentication

## Setup
First steps will be the same for basically any Next.js project:
1. Make a git repo - keep it private for now
2. Scaffold the next repo with `npx create-next-app@latest`
3. Next does the git init for us on project creation so just connect to remote and push
4. Install dependencies
5. Remove boilerplate

### Install dependencies
I installed mongodb, mongoose, nextAuth and a lib called bcrypt for hashing passwords by running:
```bash
npm install bcrypt mongodb mongoose next-auth
```
Later on I installed react-icons because I forgot to do it along with the others.

### Remove boilerplate
First go into app > page.tsx and remove the import for the next image. Then remove all the jsx in the return statement and replace it with a hello world div to check updates. Also use this as an opportunity to play with some tailwind CSS classes. 

At this point it became clear that my tailwind intellisense was not working, which was a big problem because I was planning to rely on it to help me learn tailwind classes. After some google searching I fixed the issue by opening my vs code settings.json and adding:
```json
"editor.quickSuggestions": {  
   "strings": true  
},  
"css.validate": false,  
"editor.inlineSuggest.enabled": true
```

Next in the layout.js page in the app directory and add title and description metadata.

Then in the globals.css file remove all the rules, leaving only the tailwind imports, then add a rule for html, body, :root that sets  height to 100%. Go to the tailwind config and remove the extend object that was used for the background image gradient in the demo page.

Create new folder outside the app directory, ie top-level, called components, to house our re-usable components. Create top level folder called models for MongoDB models and one called utils for utility functions. Then create a top-level file called .env and add it to the gitignore.

At this point I also took the tailwind config and globals.css from [this](https://gist.github.com/adrianhajdin/6df61c6cd5ed052dce814a765bff9032) github gist to get started with some useful compound tailwind classes, and moved the globals.css to it's own top level styles folder.

## Start building pages and components
Setup some basic filler for the home page in the app > page.js file. a title, tag line and some introduction text. Use tailwind styling and the compound styles set up in the github gist that I copied. Edited some of the gist styling too.

Create component file for a navbar component, give it a home themed logo from the react-icons library, and add the Nav comonent to the app > layout.js so that it will persist on any rendered page, not just the home page in app > page.jsx.

Create a new components in the components directory for a login card, include it in the homepage under the intro text. Deleted the custom styling for a "Prompt card" in the styles from the gist, and added new ones from tailwinds examples website to make a horizontal card for the login card component.

Create a folder in the components folder for the budget component and it's sub components, and start porting over the functionality for the budget app that I've already built. Create a budget folder under the app directory with a page.js file, where the componenets will be imported and arranged. Now we can go to localhost:3000/budget and see the layout for the budget app part of the application as we build it out. Do the same for the todo and shoppinglist pages.

Create a top level contexts directory to manage the context providers for the various pages.

## Implement the budget tracker from previous project
The previous budget tracking app project mentioned earlier will be implemented as part of this project. Eventually it will be hooked up to a database to give it persistence, but for now I am just looking at migrating the layout and clientside functionality.

The styling will have to be completely overhauled, as this project is using tailwind css whereas the original implementation used bootstrap. Currently just worrying about the basic layout, next I will do the logic, then at then end do the styiling. Unless styling is needed to make it readable while I test the logic.

### Basic layout and structure for budget component
The budget functionality will be a discrete route. It will likely use the same global layout from the app directory. The app > budget > page.js file will fill the same role here as the App.jsx file in the original standalone implementation, dictating the tree of sub components. The sub components can basically be laid out the same way as the original, but in the top level components directory of this project.

#### Update as of 24/5/23
Ended up porting over all of the functionality of the standalone budget project pretty much as is, replacing bootstrap styling with basic placeholder tailwind styles. The functionality is all working and full style overhaul will come later when the rest of the sites functionality has been properly stood up and tested. 

The budget route is currently fully in line with the standalone in terms of functionality, which means the next meaningful change will be the addition of permanence when the DB is up and running. 

## Adding auth context
While I am planning to use the nextAuth module that I've already included as a dependency to handle user authentication and white listing eventually, I realised that I want the navbar, the login card and the component menu to all have access to the state of user authentication. To address this I have implemented a simple context file with a reducer that accepts login and logout action types, and sets a a boolean for the login status of the current user in a global state object. This allows the login card to query the context and either display itself or a component menu that will be like a homepage nav to the main function pages.

The component menu will be implemented tonight as a module that will be imported into the login card and conditionally displayed as a child.
