#projects 

This is a simple budget app built by following along with [this](https://www.youtube.com/watch?v=aeYxBd1it7I&list=PLlrrMbxpJkWxu6e6n5BvB3rMf5kJYNcgu&index=6) tutorial, and tweaked/re-written as I go along for study.

## Technologies
For the app itself
- React/JSX
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

## React-icons install
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

## Starting to build
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