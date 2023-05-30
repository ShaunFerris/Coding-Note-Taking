#projects 

This is my first attempt at a full-stack app that I will actually deploy and use in real life. My previous set of project notes for a standalone react budget app were very detailed and step by step, like explicit instructions. This is because I was following a tutorial pretty closely. 

This project however is almost entirely self directed so my notes on it here are likely to be much more chaotic and disordered.

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

#### Update on budget page as of 24/5/23
Ended up porting over all of the functionality of the standalone budget project pretty much as is, replacing bootstrap styling with basic placeholder tailwind styles. The functionality is all working and full style overhaul will come later when the rest of the sites functionality has been properly stood up and tested. 

The budget route is currently fully in line with the standalone in terms of functionality, which means the next meaningful change will be the addition of permanence when the DB is up and running. 

## Adding auth context
While I am planning to use the nextAuth module that I've already included as a dependency to handle user authentication and white listing eventually, I realised that I want the navbar, the login card and the component menu to all have access to the state of user authentication. To address this I have implemented a simple context file with a reducer that accepts login and logout action types, and sets a a boolean for the login status of the current user in a global state object. This allows the login card to query the context and either display itself or a component menu that will be like a homepage nav to the main function pages.

The component menu will be implemented tonight as a module that will be imported into the login card and conditionally displayed as a child.

## Update 25/5/23
Implemented the components card menu and added it as a conditional child of the login card component, using the Next Link react element for routing to the component pages.

Now I will link up the log out button to complete the placeholder authentication/navigation system, which will be used to get around the site for testing as I implement the mvp of the other features. This functionality is achieved by writing a function that calls dispatch with an action object containing the "LOGOUT" type, and setting that function as the onSubmit trigger of the logout button element.

Also went back an edited the card menu component to include gifs representing each of the page routes, a piggy bank for the budget etc. I used gifs from giphy, and used ezgif.com to generate a version of the gif that is static and one that is animated. Then I used a local state variable to track whether the div containing the image and text is hovered, and when hovered display the animated one, else display the static. It makes for a cool effect where each menu item has a picture that starts moving when hovered.

### Planning and skeletonising the todo list route
The main page for the todo list should inherit from the global app directory layout, so that it shows the navbar and the main content conforms to the main element.

Inside that page component should be a permanent title and full width input field, with placeholder text prompting the user to enter a todolist task.

Below that will be a pending task list and completed task list, as separate components. Each of these components will make use of a todolist context that tracks list entries, and renders them if the flag matches theirs. Ie, the listitem objects in state will be something like this:
```jsx
const todoItems = {
	{
		id: uuidv4(),
		name: "taskname",
		pending: true,
		user: "Admin",
		timeStamp: new Date()
	}
};
```
The todoItem will be rendered in the pending component if it's pending property is set to true, and in the completed component if it is set to false.

## Update 26/05/2023
Continuing on with work on the todolist page route, I have divided the layout into three components so far, all of which are styled as cards on a flex layout:
1. TodoAddForm - a simple card with full width of the section, holding a text input bar and submit button for adding tasks to the pending list
2. PendingLIst - a half width vertical card that will display the pending tasks as they are added
3. CompleteList - same styling as the pending list but will display the tasks that have been marked as complete, will have additional functionality to delete tasks permanently, either one by one or all at once.

Functionality to add tasks to the list will be tied to the submit button of the todo add form component, and will go through context to update the pending list. Then the pending list will have the functionality to mark a task as complete which will re-render both list components moving the task to the complete list.

At this point I realised that the initial state in the Todo context was set up to only represent a single todo item, so I rewrote it to hold two properties, a completed tasks list and a pending tasks list, both initialized to empty arrays.

Next I decided to scaffold another component for the todo route, a task list element. Given that the pending and completed list elements will both need to be able to display items, it makes sense that the code to handle that be a reusable component.

Took a little detour here to do some style fixes to the budget page. I liked the floating card menus used on the home page and for the layout of the TODO page so far, so I added a card around the expense list and it does look nicer. Wll come back to restyling it and to building the TODO list component tomorrow.

## Update 30/05/2023
Took long weekend off and coming back into working on this project with a clearer head. 

### DB Setup
Today I have decided to work on getting the database sorted out and connected to the project, so that I can start learning how to work with MongoDB, Mongoose, and Mongo Atlas.

Roughly following [this videos instructions at 1:25:26](https://www.youtube.com/watch?v=wm5gMKuwSYk&list=PLlrrMbxpJkWxPT2xOCCg0naboy_il9JvU&index=16) and also consulting the [docs](https://www.mongodb.com/developer/languages/javascript/nextjs-with-mongodb/).

First step was to create a file called database.js in the utils directory of the project. In this file we import mongoose from mongoose, and initialize a variable called `isConnected` to false. Then we need to export an async function for connecting to the database. At this point the database.js file looks like this:
```js
import mongoose from "mongoose";

let isConnected = false;

export const connectToDB = async () => {
    mongoose.set("strictQuery", true);

    if (isConnected) {
        console.log("MongoDB is already connected");
        return;
    }

    try {
        await mongoose.connect(process.env.MONGODB_URI, {
            dbName: "homeHub",
            useNewUrlParser: true,
            useUnifiedTopology: true
        });
        isConnected = true;
        console.log("MongoDB Connected");
    } catch (error) {
        console.log(error);
    }
};
```

Notice that I have pointed to the .env file for the URI that will be used to connect to the database, but we don't have that yet. I'm going to use MongoDB Atlas, a cloud hosting service with a forever free tier, to host an instance of MongoDB for this project. The next step then is to make an account there, get a DB instance and paste the URI into the projects .env file. The env should already be on the .gitignore so we shouldn't have to worry about leaking secrets.

I made an account on MongoDB Atlas, and set up a free database instance. Added my own local IP address, as well as 0.0.0.0/0 so that connections to it can come from anywhere. Next I clicked connect on the db, went to drivers, and copied the URI that needs to go into the homeHub .env file.

### Auth setup with NextAuth
Next, I have decided to setup the authentication with nextAuth, which will make use of the DB instance we just setup to store sessions.

