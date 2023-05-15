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
	git push -u origin main
	```
5. Install bootstrap and font awesome icons.

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

### Font Awesome install
First you need to install the SVG core package:
```bash
npm i --save @fortawesome/fontawesome-svg-core
```
Then you can add either or both of the free icon style packages:
```bash
# Free icons styles
npm i --save @fortawesome/free-solid-svg-icons
npm i --save @fortawesome/free-regular-svg-icons
```
Then add the react component:
```bash
npm i --save @fortawesome/react-fontawesome@latest
```
Then you can include an icon and test it by adding it to the test buttons we added to the app with bootstrap above. Import the coffee icon for testing like so:
```jsx
  import ReactDOM from 'react-dom'
  import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
  import { faCoffee } from '@fortawesome/free-solid-svg-icons'
```
Then add an icon using a custom react html element like this:
```jsx
const element = <FontAwesomeIcon icon={faCoffee} />
```
 Now your live server preview should look like this:
 ![[Pasted image 20230515125449.png]]
 Project setup and dependancy installs are now done! Go ahead and push to remote to save progress.

## Further setup
