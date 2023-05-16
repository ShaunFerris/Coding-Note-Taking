#projects 

This is a simple budget app built by following along with [this](https://www.youtube.com/watch?v=aeYxBd1it7I&list=PLlrrMbxpJkWxu6e6n5BvB3rMf5kJYNcgu&index=6) tutorial, and tweaked/re-written as I go along for study.

NOTE: This ended up being a blow by blow of what the tutorial has you do, future Project notes will be more free form notes of thought process and design, but at this point I am still very much learning React so a thorough step by step seemed useful to have.

## Technologies
For the app itself
- React/JSX - Specifically we will be using the context api to cotrol a central state to be used by components.
- Vite
- Bootstrap
For dev
- git
- vscode + some extensions
- npm

## Learning points to cover
- Breaking down a UI into react components
- Work with state using the context API

## Process
The first steps for making this simple app are the usual steps for creating a new project with vite:
1. Make a new repo on github - don't add a README or a license just yet
2. Run `npm create vite@latest` in our coding projects directory
3. Follow prompts then run `npm install` to install react, rollup and other dependancies and `npm run dev` to start the dev server so you can check it all worked properly
4. Setup the git repo you made earlier as the remote repo for this project:
	```bash
	git init #Creates the .git folder and sets this dir up as a local repo
	git add . #Add all project files not on .gitignore to local staging
	git commit -m "<msg>" #Commit to local branch
	git branch -M main #Rename master to main to match remote
	git remote add orign <repo url from github>
	git push -u origin main #Mark the remote as upstream and push to it
	```
1. Install bootstrap and font react-icons for styling, and UUID for generating and tracking unique ids.

### Bootstrap install
In the project root directory run:
```bash
npm install react-bootstrap bootstrap
```
Then after install import bootstrap 5 into main.jsx:
```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
import './index.css'
import 'bootstrap/dist/css/bootstrap.min.css';

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```
You can remove index.css and it's import in main.jsx if you are using only bootstrap styling - for this project that is what we will do for now. We can always create a new css file if we want to add custom styling later.

Test that it worked by importing and using a bootstrap button:
```jsx
import { useState } from 'react';
import reactLogo from './assets/react.svg';
import './App.css';
import { Button } from 'react-bootstrap';

function App() {
  const [count, setCount] = useState(0);

  return (
    <div className="App">
      <div>
        <a href="https://vitejs.dev" target="_blank">
          <img src="/vite.svg" className="logo" alt="Vite logo" />
        </a>
        <a href="https://reactjs.org" target="_blank">
          <img src={reactLogo} className="logo react" alt="React logo" />
        </a>
      </div>
      <h1>Vite + React with ypescript + Bootstrap 5</h1>
      <div className="card">
        <button onClick={() => setCount((count) => count + 1)}>
          count is {count}
        </button>
        <p>
          Edit <code>src/App.tsx</code> and save to test HMR
        </p>
      </div>
      <p className="read-the-docs">
        Click on the Vite and React logos to learn more
      </p>
      <Button variant="primary">Primary</Button>{' '}
      <Button variant="secondary">Secondary</Button>{' '}
      <Button variant="success">Success</Button>{' '}
      <Button variant="warning">Warning</Button>{' '}
      <Button variant="danger">Danger</Button>{' '}
      <Button variant="info">Info</Button>{' '}
      <Button variant="light">Light</Button>{' '}
      <Button variant="dark">Dark</Button> <Button variant="link">Link</Button>
    </div>
  );
}

export default App;
```

### React-icons install
Run `npm install react-icons --save` to install this package. To use the icons, import and deploy them with this syntax:
```jsx
import { FaBeer } from "react-icons/fa";
class Question extends React.Component { 
	render() { 
		return <h3> Lets go for a <FaBeer />? </h3>
	}
}
```
Now insert beer icons into your app to test that it worked. Now your live server preview should look like this:
![[Pasted image 20230515132714.png]]

### UUID install
This one is simple, just run `npm install uuid`, nothing else is required. For info on what exactly this package is and how it works see [here.](https://www.npmjs.com/package/uuid)

Project setup and dependancy installs are now done! Go ahead and push to remote to save progress.

## Further setup - Cleanup boilerplate
Now we will remove the boilerplate provided by vite that we don't need. Delete App.css and index.css in the src directory, and the vite logo.svg in the public directory.

Go into main.jsx and delete the import for index.css, then go into App.jsx an delete everything to start from scratch.

## Starting to build - Mockup the components of the UI
Start by importing react, and creating a basic App function that will render a bootstrap button with the beer logo again, just for testing. Don't forget to add the line `export default App;` to the end of the App.jsx file.

Assuming this test has been passed and we were able to render a black page with a single button correctly styled with bootstrap and showing the icon, we will now delete these elements.

Now we begin building the layout UI elements with hardcoded data. This will help us visualise the state objects and the different data we will need to add. Open braces on the return statement for the app function so that our JSX looks nice, and open a bootstrap container div to wrap everything. Inside that make:
1. An H1 with class mt-3 (margin top 3) and the title we want displayed at the top
2. A mt-3 row div containing 3 col-sm divs, each of which get a custom jsx element for a component to come

### Creating the skeleton of our first components
Create a new directory in the source directory called components and add .jsx files for the first three components: Budget, Remaining, and ExpenseTotal. Remember  to start with a capital and use camelCase on all the names.

To create a basic components, import React, create the component function having it return a div  and a span to contain the data. Use bootstrap alert classes on the divs to make the blocks look nice, with different alert styles for different colors. See [here](https://getbootstrap.com/docs/4.0/components/alerts/) for the different alert colors.

For now the data should be hard-coded, we can get it from context in the future. Finally export default the component function and import it in App.jsx - for example:
```jsx
import Budget from './components/Budget';
```
Do this for all three of the components and add them to the bootstrap col divs in App.jsx. Now we should have resposively laid out boxes for our three budget categories. The logic to track and update display of budget, amount remaining etc will be written into the module jsx files later.

### Skeletonize the ExpenseList component
To make the skeleton of a component that will track a list of expenses, give it an H1 under the row element that contains the top three components. Make sure it's still in the container and give it top margin of 3 again. The ExpenseList component will then go in a row mt-3 -> col-sm like the earlier components.

Next mock up the ExpenseList.jsx component to return a dummy hard-coded list of expenses so we can see it in the UI. Store the expenses in an array, as objects. Each expense object should have an id, name and cost. Then return an unordered list containing js {}, within which we map the expenses array with a function that renders an `<ExpenseItem />` component to which the properties of the currently mapped expense object are passed as props.

Now we can go and build the ExpenseItem component that is needed to make the ExpensesList component work.

### Skeletonize the ExpenseItem component a child of ExpenseList
Create a new component file for this one in the same way as the others, but this time have the component function accept props. The component should return a `<li>` tag with `className=list-group-item d-flex justify-content-between align-items-center`
to give it nice bootstrap styling. The d-flex class makes it a flex item, and allows the justify and align classes to be applied.

Within the list item element, display the data from props with name first, then cost in a div. Inside the div, wrapped around cost should be a bootstrap pill badge span, with margin-right 3, then add a delete button icon with `<TiDelete size='1.5em'></TiDelete>`.  Dont forget to import TiDelete from react-icons/ti.

The finished module should look like this:
```jsx
import React from "react";
import { TiDelete } from "react-icons/ti"

const ExpenseItem = (props) => {
    return (
        <li className="list-group-item d-flex justify-content-between align-items-center">
            {props.name}
            <div>
                <span className="mr-3 badge badge-pill bg-primary">
                    ${props.cost}
                </span>
                <TiDelete size="1.5em"></TiDelete>
            </div>
        </li>
    );
};

export default ExpenseItem;
```

### Mock up the Add Expense Component
This time we won't list every detail of what was done, but the goal is to mock up the ui for a field that will accept a typed name and cost, then a button can be clicked to add that expense. Again the logic will come later, this is just a mock up. The finished component looked like:
```jsx
import React from "react";
import { Button } from "react-bootstrap";

const AddExpenseForm = () => {
    return (
        <form>
            <div className="row">
                <div className="col-sm">
                    <label for="name">Name</label>
                    <input
                        className="form-control"
                        required="required"
                        type="text"
                        id="name"
                    ></input>
                </div>
                <div className="col-sm">
                    <label for="cost">Cost</label>
                    <input
                        className="form-control"
                        required="required"
                        type="text"
                        id="cost"
                    ></input>
                </div>
                <div className="col-sm mt-auto">
                    <Button variant="primary">Save</Button>
                </div>
            </div>
        </form>
    );
};

export default AddExpenseForm;
```

## Define the context
Now that the UI has been split up into components, and those components have been mocked up with hard-coded placeholder data, we can begin to work on the context handling of our budget app.

The context will hold the global state of the app which other components will use. Create a file called AppContext.jsx in a new directory called context under the src directory.

Import the createContext builtin from react, intialize an initial state variable, and create a context. The code at this stage looks like this: 
```jsx
import { createContext } from "react";

const initialState = {
    budget: 2000,
    expenses: [
        {id: 12, name: shopping, cost: 40},
        {id: 12, name: shopping, cost: 40},
        {id: 12, name: shopping, cost: 40},
    ]
};

export const AppContext = createContext();
```

Next we need to create provider, which is the entity that holds the state and passes it to our components. In order to hold the state, it needs to use the `useReducer()` hook, so we should at the to our imports. 

A quick reminder, the `useReducer` hook is similar to the `useState` hook. It will provide us with the current state, and also a function to update the state. It needs to be passed a reducer funciton and the initial state. 

The reducer function is responsible for creating the new state object from the initial state it is passed.  To write a reducer, write an arrow function that accepts the current global state passed to it by react, and an action, passed to it by our dispatch function. The reducer will use a switch statement to determine how to update the state based on the action type. For now, just give it a default case that returns the state unchanged. We will add new action cases to the reducer as we need them going forward.

Now we just need to finish the AppProvider by having it return some state objects so that the connected components can have access to them.

After importing useReducer, writing a reducer function, and writing an AppProvider function to which the reducer is connected, the AppContext.jsx file looks like this:
```jsx
import { createContext, useReducer } from "react";

const AppReducer = (state, action) => {
    switch (action.type) {
        default:
            return state;
    }
};

const initialState = {
    budget: 2000,
    expenses: [
        { id: 12, name: shopping, cost: 40 },
        { id: 12, name: shopping, cost: 40 },
        { id: 12, name: shopping, cost: 40 },
    ]
};

export const AppContext = createContext();

export const AppProvider = (props) => {
    const [state, dispatch] = useReducer(AppReducer, initialState);

    return (
        <AppContext.Provider value={{
            budget: state.budget,
            expenses: state.expenses,
            dispatch
        }}
        >
            {props.children}
        </AppContext.Provider>
    )
};
```

Now that a basic context for our app has been set up, we need to connect it to the app, so import AppProvider into the App.jsx file, then wrap all the exisitng code in the App functions return statement in an `<AppProvider>` JSX element.

Now all the components that we have created so far that are rendered in the return of our App function, like Budget, Remaining etc, have access to the context because they are children of the AppProvider. Now we can start connecting our components to the context to display the data we need from context in each component.

## Using context in our components

### Budget component
Going back to our Budget component, we will replace the hardcoded data with context provided state data. This is what the component looks like before:
```jsx
import React from "react";

const Budget = () => {
    return (
        <div className="alert alert-secondary">
            <span>Budget $1000</span>
        </div>
    );
};

export default Budget;
```
First we need to import the AppContext from AppContext.jsx, and the useContext hook from React.
Next, inside the budget component function, use destructuring to get the budget state value out of the context with the useContext hook, and assign it to a variable called budget. Finally, use this variable in the return statement of the budget component in place of the previously hardcoded value. The finished budget component with the budget data coming from the global state object through the context API looks like this:
```jsx
import React, { useContext } from "react";
import { AppContext } from "../context/AppContext";

const Budget = () => {
    const { budget } = useContext(AppContext);
    return (
        <div className="alert alert-secondary">
            <span>Budget ${budget}</span>
        </div>
    );
};

export default Budget;
```
And now the value for budget set in the initial state  constant in the AppContext.jsx file will show properly on the web page.

### ExpenseList component
Again we import the AppContext from AppContext.jsx, and the useContext hook from React. Then we can simply replace the hardcoded expenses array with the expenses array in the global context object defined in AppContext.jsx, using a destructuring assignment to the useContext hook, like we did above. That's it for this component!

## Adding to the global state object through context
Now that we have updated the Budget and ExpenseList components to consume data from the global state object, we will look at the AddExpenseForm component, which by it's very nature will need to add data to the global state object through context, to be consumed in other components.

This will involve **dispatching** an **action** into context and updating the expenses list that lives in there. 

Before we can save the new expense to state we need to know which values the user has entered into the 'name' and 'cost' inputs on the form. We are going to use some local state objects which will hold the values for each of these fields. To do this we need to import the useState hook, and then use that to create local states for the name and cost:
```jsx
import React, { useState } from "react";
import { Button } from "react-bootstrap";

const AddExpenseForm = () => {
    const [name, setName] = useState('');
    const [cost, setCost] = useState('');

    return (...);
};
```
Next we go to the name input and give it the name variable from the useState statement as a prop. Then also give it an onChange event handler prop that uses the setName function from useState to set name to the events target value. Do the same for the cost, and now the name and cost local state objects will update with the input fields. For clarity, the name input fields looks like this now:
```jsx
<input
    className="form-control"
    required="required"
    type="text"
    id="name"
    value={name}
    onChange={(event) => setName(event.target.value)}
></input>
```

Next we need to add an onSubmit event function to the form that gets called whenever the button is clicked. For now just use a preventDefault() call to stop the page from being reloaded when the button fires, and then fire an alert that calls out the name and cost values. This will allow us to test that it is working as intended.
```jsx
    const onSubmit = (event) => {
        event.preventDefault();
        alert("name " + name + " cost" + cost);
    };
```
Don't forget to add `onSubmit={onSubmit}` to the form tag.

With the test version of the onSubmit function working properly, we can now update it to dispatch an action to tell the conext to update the expenses list. 

Start by importing the context and useContext hook, then destructure the dispatch function from context with the useContext hook.  Then replace the alert line in the onSubmit function with an expense object that takes a name and cost property from the name and cost local state variables. Use parseInt() to turn the cost state variable to an integer, as it is a string when taken in by the form input. We will also add an id to the added expense by assigning the id property to uuidv4(), after importing v4 as uuidv4 from uuid. This will generate a random id for each expense object added. 

Now, still in the onSubmit function, call the dispatch function with `{ type: ADD_EXPENSE and payload: expense }`. The type is what the reducer will use to decide which case to use to update the state, so we will need to go into AppContext.jsx and add a new case to the reducer.

The new case in the reducer should trigger when action.type matches ADD_EXPENSE and should return a copy of the current state where expenses is overridden with a copy of expenses, plus the payload added to the end, like this:
```jsx
const AppReducer = (state, action) => {
    switch (action.type) {
        case 'ADD_EXPENSE':
            return {
                ...state,
                expenses: [...state.expenses, action.payload]
            };
        default:
            return state;
    }
};
```
TO RECAP: With this work done, when we enter a name and cost into the new expense form, and click the save button, the onSubmit() function is called. It creates a new expense object with name and cost equal to what was entered, and a random id generated by uuid. These are the three properties, id, name and cost that we need in state to display an expense. We then dispatch an action of type: "ADD_EXPENSE" with the newly created expense object as payload. The dispatch action goes off to AppContext where it is mapped to the action object taken by the reducer which returns an updated state. Whenever the reducer returns, it updates the state in the AppProvider. Because the state in the AppProvider has changed, the value that is given to the components has also changed, which tells the components to re-render.

Go through the above, looking at the source code and make sense of it. It is good study to understand.

## Calculating the remaining budget from state
In the Remaining.jsx file we want to calculate the value to render on the ui. We can do this by getting expenses and budget from the global state through context, then reducing the expenses array to a total expenses cost, and subtracting this from budget in the span tag that renders the text an value.

Finally, use a ternary expression to change the alert-success class to alert-danger if the value of remaining is below 0. Notice the template literal string used in the className.
```jsx
import React, { useContext } from "react";
import { AppContext } from "../context/AppContext";

const Remaining = () => {
    const { budget, expenses } = useContext(AppContext);
    
    const totalExpenses = expenses.reduce((total, expense) => {
        return (total + expense.cost);
    }, 0);

    const alertType = totalExpenses > budget ? 
        "alert-danger" :
        "alert-success";

    return (
        <div className={`alert ${alertType}`}>
            <span>Remaining: ${budget - totalExpenses}</span>
        </div>
    );
};

export default Remaining;
```

We will next do a similar process to add context to the ExpenseTotal component, but this won't be documented here as this note is already way too long.

## Deleting expenses from the array in the global state object
ALMOST DONE! Just a few more features to implement properly. One of the last ones is to allow the delete buttons on our list of expenses to acutally remove the corresponding expense object from the expenses array in the global state object, triggering a context change and a re-draw of the list element.

