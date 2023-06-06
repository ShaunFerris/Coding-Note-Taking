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

First, I made a new component called AuthProvider.jsx, and set it up as a standard React functional component. Import SessionProvider from nextAuth, give the component a props object with children and session, and have the component return the session provider wrapped around the children. At this point the AuthProvider component looks like this:
```jsx
"use client";

import { SessionProvider } from "next-auth/react";

const AuthProvider = ({ children, session}) => {
    return (
        <SessionProvider session={session}>
            {children}
        </SessionProvider>
    );
};

export default AuthProvider;
```

Next we need to go to layout and wrap the AuthProvider around everything in the body element, so that all content on all pages will be wrapped by it, and have access to the session status.

At this point I realized that I already have a component called AuthProvider from my stand in authentication context. For now I am just going to rename the new component to NextAuthProvider and carry on, probably removing the stand in AuthProvider later once the NextAuth is properly set up.

NextAuth doesn't just use the front end components of the app, it also uses the api backend endpoints. So this will be an opportunity to set this up and learn. In the app directory, I created an api directory, with an auth directory inside of that. Finally, inside the auth directory, create a dynamic directory called ...nextauth. So the file structure route looks like this:
```
app -> api -> auth -> [...nextauth]
```

Inside the ...nextauth dynamic directory, I created a file called route.js. In this file I will setup the providers for authenticating users, in this case I will just be using google for now. In this file import NextAuth and GoogleProvider from next auth and create a handler by calling NextAuth as a function and giving it an options object. The options object we give it will have three entries, a providers array, a signIn async function and a session async function. Finally, the handler function should be exported as GET and again as POST. At this point the route file looks like this:
```jsx
import NextAuth from "next-auth/next";
import GoogleProvider from 'next-auth/providers/google';

const handler = NextAuth({
    providers: [
        GoogleProvider({
            clientId: '',
            clientSecret: ''
        })
    ],
    async session({ session }) {

    },
    async signIn({ profile }) {

    }
});

export { handler as GET, handler as POST };
```

At this point the next step is to go to google console and get credentials to add into the clientId and clientSecret properties in the GoogleProvider options object. After logging into console.cloud.google.com, I created a new project called homeHub and selected it, then went to APIs and services in the sidebar menu, and finally to oAuth consent screen. Then go through and fill in the form. 

Go to credentials, create credentials, oAuth, clientId. Select web app as the type, and add `https://localhost:3000` as the authorized javascript origin and the authorized redirect URI and then click create.

Now we can copy the clientId and client secret and add them to the .env file in the project, then include them in route file using `process.env.GOOGLE_ID/CLIENT_SECRET`. Then test that the credentials are being pulled from the .env file correctly by console logging them as an object from the route.js file, running the dev server and checking the browser console. The logs are not coming up in the browser console, but they are appearing every couple of seconds in the terminal from which the dev server is running. 

At this point remove the console log of the credentials and continue developing the nextAuth route.

### Auth setup - session and sign in functions
Because every next.js route is a serverless route, the sign in and session functions are Lambda functions that open up only when they get called. So every time it gets called the server will spin up and make a connection to the database. This is where the connectToDB function from the utils/database.js file from earlier will come into play. 

Import the function and call it as an await inside a try block under the signIn function. Then we want to check if the user already exists, and if they don't then create a new user and add them to the DB, and then return true. Under the catch block we should console log the error, and return false. At this point the function looks like this:
```js
    async signIn({ profile }) {
        try {
            await connectToDB();

            //check if the user already exists

            //if not, add new user to the db

            return true;
        } catch (error) {
            console.log(error);
            return false;
        }
    }
```

To implement the add user function we need to implement a database model, based on which the user document will be created and added to the MongoDB document database. 

Create a new file called user.js inside the models directory that was made at the outset of the project. Import schema, model and models from mongoose, and create a userSchema as a new instance of the schema class, with an options object passed in that defines the data we want associated with each user added to the DB.

We want an email, a username and an image, with the email and username being required, the email being unique and the username being 8-20 alphanumeric characters and unique. This can be achieved with a regex.

Finally we need to assign the User variable to a model called user and constructed from the userSchema, and export that variable. Because next api routes are serverless though, we need to check if a model named user already exists in the models object provided by mongoose so that we don't redefine it everytime the serverless route is called. The finished code in user.js looks like this:
```js
import { Schema, model, models } from 'mongoose';

const UserSchema = new Schema({
    email: {
        type: String,
        unique: [true, "Email already in use"],
        required: [true, "An Email is required"]
    },
    username: {
        type: String,
        required: [true, "A username is required"],
        match: [
            /^(?=.{8,20}$)(?![_.])(?!.*[_.]{2})[a-zA-Z0-9._]+(?<![_.])$/,
            "Username invalid, it should contain 8-20 alphanumeric letters and be unique!"
        ]
    },
    image: {
        type: String
    }
});

const User = models.User || model("User", UserSchema);

export default User;
```

Going back to the route file in the auth api, import the user model and use the findOne method of the model object to check if the user exists, and the create method to make a User model entry in the database if they do not. 

Now the signIn function is finished, and we need to flesh out the session function so that we can keep track of user sessions. We do this by implementing a variable called sessionUser set to an await function that uses the findOne method of the User object to match the email of the current user, and update the session.user.id, and return the new session. 

At this point the auth route file is finished and looks like this:
```js
import NextAuth from "next-auth/next";
import GoogleProvider from 'next-auth/providers/google';
import { connectToDB } from "@/utils/database";
import User from "@/models/user";

const handler = NextAuth({
    providers: [
        GoogleProvider({
            clientId: process.env.GOOGLE_ID,
            clientSecret: process.env.GOOGLE_CLIENT_SECRET
        })
    ],
    async session({ session }) {
        const sessionUser = await User.findOne({
            email: session.user.email
        })

        session.user.id = sessionUser._id.toString();
        return session;
    },
    async signIn({ profile }) {
        try {
            await connectToDB();

            //check if the user already exists
            const userExists = User.findOne({
                email: profile.email
            });
            //if not, add new user to the db
            if (!userExists) {
                await User.create({
                    email: profile.email,
                    username: profile.name.replace(" ", "").toLowercase(),
                    image: profile.picture
                })
            }

            return true;
        } catch (error) {
            console.log(error);
            return false;
        }
    }
});

export { handler as GET, handler as POST };
```

Now before testing we need to setup some nextAuth env variables. The URLs will be set to local host for now and changed when we are ready to deploy. The secret is a random string generated with open ssl as described in the nextAuth docs. 

At this point we can test the site out. We immediately see an error caused by the top-level-await experiment not being enabled, this can be fixed by enabling it in the next config file. At this point the app compiles and runs fine, but in order to actually test that the route and DB setup worked properly, we need to go back and use the real session data in our code wherever the logged in status is checked.

### Hooking up login/logout functionality through NextAuth
Going back to the LoginCard component that I wrote in the past, which conditionally renders a login prompt or the homepage card menu based on the AuthContext mockup, I now remove the import of that AuthContext, and the use context call and dispatch function. Then is use the NextAuth use session hook to get the session data, and replace to isLoggedIn conditional in the return function with a session?.user conditional.

This appears to work, but the login button now shows an error, becuase we need to add an authorized route to the google oAuth settings. Under the authorised callback URI settings in the google cloud console we add the URI: `http://localhost:3000/api/auth/callback/google`

Then wait a while and see if it worked, it did not work immediately but the console specifies that settings changes can take a few minutes to a few hours to be implemented. NOTE: It actually turned out that it wasn't working becuase I had used https instead of http in the redirect URI.

With the login functionality working and hooked up to the button on the login card component, I will now move onto implementing log out functionality in the navbar, and try to render the users google account avatar in the nav when they are logged in as well. 

The logout implementation was really easy, just setting up the useSession call the same way as in the login card, then adding the NextAuth sign out function to the onClick trigger of the sign out button, and replacing to isLoggedIn conditional as before. 

To render the google accounts avatar image next to the sign out button, I used a next Link tag that points to a profile route (Made just now, currently empty), wrapping a next Image tag with source set to {session?.user.image}.

## Update 31/05/2023
Started of today by reworking some of the styling for the budget page, adding floating cards around the sections to bring it more in-line with the other pages on the site design wise.

Wrote a basic mongoose schema for the budget data we want to keep track of.

Discovered today that the user data is not persisting in the database, this is due to an error in the route, where the session and sign in functions are supposed to be in a callbacks object under the providers object, instead of out on their own as entries into the NextAuth options object. This fixed the problem that I had where nothing was being stored in the database, because the connection was never happening, but introduced another problem. The app is now connecting, and has created the user collection on the db, but login functionality is broken, and I'm getting a JWT error:
```
[next-auth][error][JWT_SESSION_ERROR] 
https://next-auth.js.org/errors#jwt_session_error Cannot read properties of null (reading '_id') 

....traceback....
```

After tearing my hair out for ages I managed to debug this. The JWT error was coming from the session function in the auth api route, where it tried to write the session user id as the id of the user from the database. This was failing because when the call happened the user was not written to the database. The user was not being written to the database because the userExists variable was never null, and it was never null because the await statement was missing before the User.findOne() call, causing the userExists variable to be a query object, rather than null which it would be if the query was awaited for resolution.

## Update 02/06/2023
Took yesterday mostly off from work on this project, the only work I did on it was to add a validator to the budget schema that allows either an array containing valid expense objects, or an empty array.

Now seems like a good time to evaluate the project so far and what will come next. So far I have stood up the general design and layout of most of the site, properly integrated nextAuth for user authentication and session tracking, and connected to a MonogDB instance on Atlas for storing user data, and eventually budget/todo/shopping data. 

The api for authentication and db interaction so far has been setup by closely following tutorials and docs, the next step is to learn how to hook up the db to the budget tracker and todolist - it was while researching this yesterday that I realised once again how much is left to learn. I suspect that the contexts set up for the budget/todo etc won't actually be neccessary, as the data willl be pulled on and off the db server instead. As I have not completed the context for the todo list I will be using that as my first learning attempt at writing my own API endpoint and route.

Today I have setup a schema for todolist items in the models folder, and begun adding functionality to the todoAddForm component that will attempt to fetch data from an api route in the app > api > todo > newItem directory.

To give the TodoAddForm component the ability to create a new entry in the database when a todo is added, it needs to communicate with a POST route in the api endpoint. Todo this I added an async function that uses the fetch() web api to sent a POST request to the API route in the above mentioned directory, and used that function as an onsubmit for a form element wrapped around the return JSX of the component. Then I needed to define a POST route as an async function in the route.js file.

The function for fetching data from the POST route looked like this: (It is defined inside the component, above the return).
```jsx
    const { data: session } = useSession();

    const [todo, setTodo] = useState({ name: "", complete: false });
    const [isSubmitting, setIsSubmitting] = useState(false);

    const createTodo = async (event) => {
        event.preventDefault();
        setIsSubmitting(true);

        try {
            const response = await fetch("/api/todo/newItem", {
                method: "POST",
                body: JSON.stringify({
                    task: todo.name,
                    complete: todo.complete,
                    userID: session?.user.id
                })
            });

            if (response.ok) {
                console.log("Todo task created!")
            }

        } catch (error) {
            console.log(error);
        } finally {
            setIsSubmitting(false);
        }
    };
```

And the POST route in the /api/todo/newItem directory looked like this:
```jsx
import { connectToDB } from "@/utils/database";
import Todo from "@/models/todo";

export const POST = async (req) => {
    const { task, complete, userID } = await req.json();

    try {
        await connectToDB();

        const newTodo = new Todo({
            creator: userID,
            name: task,
            complete: complete
        });

        await newTodo.save();
        return new Response(JSON.stringify(newTodo), { status: 201 });

    } catch (error) {
        return new Response(
            "Failed to create a new prompt", { status: 500 }
        );
    }
};
```

This worked great and I was able to add a test task to the database of tasks! 

Next I want to work out how to delete tasks, and how to get tasks to display in either the pending or complete lists based on the boolean completed property.

### Displaying pending and completed tasks from the database
Currently, the todo page is set up with the pending and completed task lists as seperate components, that conditionally render a statement when empty or a third TaskList component. This seems really clunky to me now so I have decided to refactor this.

The new setup will be to write a tasklist component that takes a props object of this layout:
```jsx
const TaskList = ({ title, emptyMsg, renderCondition }) => {};
```
Where the render condition is a boolean, and the list items in the database will be rendered if their complete flag matches the render condition. Then I can reuse the tasklist component for both the pending and completed lists.

I think I understand how to populate the tasklist with a fetch call to a GET api route, but I am not sure how to make it so that the list component notices when the database has had a new entry added to it and rerender. Perhaps using context and or the useEffect hook.

## Update 04/06/2023
Today I began the planned rework of the todo list components, and it was pretty straight forward to begin with. I have consolidated the PendingTasks and CompletedTasks components into a single TodoTaskList component that takes props from the parent, and that is all working nicely.

I may in future add a TodoItem component that contains the logic for updating a tasks complete status to move it between lists, but before that I want to implement the logic for fetching tasks from the server, and I think the best way to do this in such a way that all todolist components can see it is to make the api request in context, and store the list in state given to the context provider.

Todays next task is to begin writing a function in context that makes this api request, and then also make the api route itself. 

### Problems appear
I wrote a GET api route on the backend for this and it appears to work, I am able to successfully fetch the tasks data from the database. However I am running into trouble using the data through context in the other components downstream of the provider, and I suspect that this is due to the fact that fetches to the GET endpoint return promises, and at somewhere along the way I am not awaiting the resolution of that promise. It currently looks like this:
```jsx
import { createContext, useReducer } from "react";

const TodoReducer = (state, action) => {
    switch (action.type) {
        default:
            return state;
    }
};

const fetchTasks = async () => {
    try {
        const response = await fetch("/api/todo");
        const data = await response.json();
        console.log("Successfully fetched: ", data);//LOOK HERE
        return data;
    } catch (error) {
        console.log("Failed to fetch todotasks: ", error);
    }
};

const initialState = fetchTasks().then(result => {
    result;
});

export const TodoContext = createContext();

export const TodoProvider = (props) => {
    const [state, dispatch] = useReducer(TodoReducer, initialState);

    return (
        <TodoContext.Provider
            value={{
                todoTasks: state,
                dispatch
            }}
        >
            {props.children}
        </TodoContext.Provider>
    );
};
```
The `LOOK HERE` comment in the above shows where the data is successfully printed as an array of objects, but the initial state value is still showing up as a promise object.

### Possible solution to the problem
Currently I am fetching the reponse as the initial state and then passing that into the useReducer hook in the Provider. After quite a lot of prompting and correction, chatGPT has suggested something that seems worth trying, where useEffect is called to fetch the data inside the provider, and dispatch to the reducer to set the state. I am going to rewrite the context to give this a go, currently I'm not actually using the context so it shouldn't cause too many problems, even if it does not work.

This seems to have kind of worked, I am now pulling down the data, putting it into context, and using it in the task list component! The context file now looks like this:
```jsx
import { createContext, useReducer, useEffect } from "react";

const TodoReducer = (state, action) => {
    switch (action.type) {
        case "FETCH_SUCCESS":
            return {
                loading: false,
                data: action.payload,
                error: null
            };
        case "FETCH_FAILURE":
            return {
                loading: false,
                data: null,
                error: action.payload
            };
        default:
            return state;
    }
};

const initialState = {
    loading: true,
    data: null,
    error: null
};

export const TodoContext = createContext();

export const TodoProvider = ({ children }) => {
    const [state, dispatch] = useReducer(TodoReducer, initialState);

    useEffect(() => {
        const fetchTasks = async () => {
            try {
                const response = await fetch("/api/todo");
                const data = await response.json();

                dispatch({ type: "FETCH_SUCCESS", payload: data });
            } catch (error) {
                dispatch({ type: "FETCH_FAILURE", payload: error.message });
            }
        };
        fetchTasks();
    }, [state]);

    return (
        <TodoContext.Provider
            value={{
                todoTasks: state,
                dispatch
            }}
        >
            {children}
        </TodoContext.Provider>
    );
};
```
And the TaskList component where this context is used looks like this:
```jsx
import { useContext } from "react";
import { TodoContext } from "@/context/TodoContext";

const TodoTaskList = ({ title, emptyMsg, renderCondition }) => {
    const { todoTasks, dispatch } = useContext(TodoContext);

    console.log(todoTasks);

    const tasksToDisplay = todoTasks.data === null ?
        null : todoTasks.data.filter((task) => {
            return task.complete === renderCondition;
        });

    return (
        <div className='card_container_vert'>
            <h1 className='subhead_text text-center'>
                {title}
            </h1>
            {!tasksToDisplay ? (
                <p className='desc_2 text-center'>
                    {emptyMsg}
                </p>
            ) : (
                <ul>
                    {tasksToDisplay.map((task) => (
                        <li key={task.name}>{task.name}</li>//LOOK
                    ))}
                </ul>
            )

            }
        </div>
    );
};

export default TodoTaskList;
```

I think what I want to work on next is implementing a TodoItem component, that will take as props a task object from the map function at the `LOOK` comment in the above code, and display nicely the tasks name, status and creator, and maybe give the option to change the status of the task too using the reducer.

### Update from late in the same day IMPORTANT
I have got the TodoItem component up and running, and also got the function to toggle complete state and the PATCH api endpoint working! The todolist now has functinality for add todos and toggling them between the pending and completed lists. 

I suspect I may not have done all of todays work in the best possible way though, as the connectToDB function is being called over and over again. I have determined that this is because I have put state as the variable to monitor in the useEffect call for Todo Context, but if I remove state as the dependancy to this useEffect call then the lists don't dynamically update. For now it's fine but this will likely be something that needs addressing later for performance.

## Update 05/06/2023
After hacking around for an hour and a half I have managed to eliminate the infinite loop that was caused the useEffect hook both updating state and having state as a dependancy. I eventually did this by creating a `stateChange` state variable, and passing it's setter down to the list components through context, where it could be called after awaiting the response from the PATCH API call. This stateChange setter also needs to be give to the addTodoItem component.

I'm starting to think that the context implementation I'm using now is pretty hacky and weird and will likely need refactoring later, but for now I am very happy to have solved the infinite loop issue, without sacrificing real-time reloading of the todo task lists.

Next I have restyled the toggle status buttons on the todo items, and also added a similarly styled delete button for each entry in the todo task lists. Both buttons are transparent, with the toggle buttons tuning green and the deletes turning red on hover, this was super easy with tailwind which I am finally really starting to get the hang of. 

Next step will be to create an onClick function for deleting a task, and the corresponding DELETE api endpoint in the route. This ended up being pretty straightforward too! My first attempt did not work at all but after moving the endpoint for DELETE to a dynamic route and getting the task id from { params } it worked. I think I'm getting the hang of this too!

## Update 06/06/2023
First task today was to edit to TodoTaskItem component so that it displays the user who added it alongside the task name. The todo tasks in the database already have a reference to the `_id` field of the corresponding entry in the user collection, stored as the creator field. This is just the mongo id object though, not the actual entry from the user collection. What I had previously forgotten to do was call the populate method on the creator property when fetching the todos, which swaps out the id reference stored in creator with the actual object from the user collection, so that we can then get the username from the task with `task.creator.username`. Here's the edited GET route for reference:
```jsx
export const GET = async (req) => {
    try {
        console.log("GET req");
        await connectToDB();

        const tasks = await Todo.find({}).populate('creator');

        return new Response(JSON.stringify(tasks), { status: 200 });
    } catch (error) {
        return new Response("Failed to fetch todo tasks", { status: 500 });
    }
};
```

### Shopinglist route
Next I decided to work on the shopping list route, as the Todo route is basically finished at this point, which is exciting.

I toyed with the idea of writing the whole thing as one component, but ended up deciding to structure is the same basic way that I did the Todo list, with the page displaying a form for adding items which posts to the db, and a list of those items pulled from the db. This keeps the logic for fetching the POST and GET routes seperate from each other.

I started by laying down a section in the page.js file, giving it a heading pulling in the form component. In the future I should maybe look into reusing a single form component across pages.
I then built a form component that looks very similar to the todolist one, but doesn't go full width, I want the shopping list to feel simpler and more compact. The logic for fetching the POST route was also pretty much the same as in the todolist. 

At this point I'm going to stop noting down a blow by blow of what I did because I think building this will be pretty much just following what I did with the todo list route.