#webdevSpecific 

The FSO course content on react starts by teaching the core concepts of React alongside building a simple react app. They start by spinning up an example react app repo called "part1" with vite:
```bash
npm create vite@latest part1 --template react
```
Note: I usually use npx for this rather than npm to avoid having create installed locally.
\
They next follow the usual steps of cd'ing in, running `npm install`, and starting the dev server with `npm run dev`.

Next, they trim down the entrypoint code by removing vites boilerplate untill; the `main.jsx` and `App.jsx` files look like this:
```jsx
//main.jsx
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

```jsx
//App.jsx
const App = () => {
  return (
    <div>
      <p>Hello world</p>
    </div>
  )
}

export default App
```

They briefly mention that you could also use "create-react-app" for this course, but fuck that because it is deprecated and bad.

## Components
Note right off the bat that React components must be named with the first letter capitalized. They also usually need to have one root element, i.e: one html element or react component that wraps all the others. If you need to, you can always wrap your parallel level elements in a fragment `<></>`.

The file `App.jsx` now defines a single React component named "App". The command on the final line of `main.jsx` renders the contents of the "App" component into the div element defined in the file `index.html` that has the id attribute "root".
```jsx
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

By default, the `index.html` file does not contain any html that is actually visible to the user, only meta, link and head tags, and the root div entry point to our react app.
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite + React</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

The FSO course materials mostly/always use ES6 arrow functions to define functional react components and assign them to constant variables. As I already know there are other ways to define components, but that's not important. 

They do point out here though that the return can be omitted for arrow functions that consiste of a single expression, i.e the following two component definitions are identical:
```jsx
const App = () => (
  <div>
    <p>Hello world</p>
  </div>
)

const App = () => {
  return (
    <div>
      <p>Hello world</p>
    </div>
  )
}
```

The function containing the component may contain any kind of javascript, eg:
```jsx
const App = () => {
  const now = new Date()
  const a = 10
  const b = 20
  console.log(now, a+b)

  return (
    <div>
      <p>Hello world, it is {now.toString()}</p>
      <p>
        {a} plus {b} is {a + b}
      </p>
    </div>
  )
}
```

Note the curly braces in the return statement to allow evalutaion of js statements and variables.

Finally note that the last line of the `App.jsx` file exports the component function. This will not always be shown in course examples but is always necessary.

## JSX
At first it seems React components return HTML, except they obviously don't because js expressions can be evaluated inside the return. Instead they return JSX, which looks like HTML but under the hood, is compiled into javascript. After compilation, the App component looks like this:
```javascript
const App = () => {
  const now = new Date()
  const a = 10
  const b = 20
  return React.createElement(
    'div',
    null,
    React.createElement(
      'p', null, 'Hello world, it is ', now.toString()
    ),
    React.createElement(
      'p', null, a, ' plus ', b, ' is ', a + b
    )
  )
}
```

The compilation is handled by [Babel](https://babeljs.io/repl/). Projects created with _create-react-app_ or _vite_ are configured to compile automatically. We will learn more about this topic in [part 7](https://fullstackopen.com/en/part7) of this course.

It is also possible to write React as "pure JavaScript" without using JSX. Although, nobody with a sound mind would do so.

In practice, JSX is much like HTML with the distinction that with JSX you can easily embed dynamic content by writing appropriate JavaScript within curly braces. The idea of JSX is quite similar to many templating languages, such as Thymeleaf used along with Java Spring, which are used on servers.

JSX is "[XML](https://developer.mozilla.org/en-US/docs/Web/XML/XML_introduction)-like", which means that every tag needs to be closed. For example, a newline is an empty element, which in HTML can be written as follows:
```html
<br>
```

but when writing JSX, the tag needs to be closed:
```html
<br />
```


## Working with multiple components
Componenets can be used multiple times, and can include other components in their returns, provided that the child component is either imported or defined in the same file.
```jsx
const Hello = () => {  
  return (    
    <div>
      <p>Hello world</p>    
    </div>  
  )
}

const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello />    </div>
  )
}
```
Writing components with React is easy, and by combining components, even a more complex application can be kept fairly maintainable. Indeed, a core philosophy of React is composing applications from many specialized reusable components.

Another strong convention is the idea of a _root component_ called _App_ at the top of the component tree of the application. Nevertheless, as we will learn in [part 6](https://fullstackopen.com/en/part6), there are situations where the component _App_ is not exactly the root, but is wrapped within an appropriate utility component.

## Props: passing data to components
Props are properties that the component takes as an attribute when it's element is used in JSX, and are defined by giving the functional component definition an object as a parameter.
```jsx
const Hello = (props) => {  return (
    <div>
      <p>Hello {props.name}</p>    </div>
  )
}
```

The props parameter that the "Hello" functional component takes is an object, which has fields corresponding to all the "props" the user of the component defines.

There can be an arbitrary number of props and their values can be hard-coded strings or the results of js expressions. An example of using two props in the "Hello" component would look like this:
```jsx
const Hello = (props) => {
  console.log(props)  
  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
    </div>
  )
}

const App = () => {
  const name = 'Peter'  
  const age = 10
  return (
    <div>
      <h1>Greetings</h1>
      <Hello name='Maya' age={26 + 10} />      
      <Hello name={name} age={age} />    
    </div>
  )
}
```

Note that the component receiving props can also destructure the props needed directly out of the props object like this:
```jsx
const Hello = ({name, age}) => {
  console.log(props)  
  return (
    <div>
      <p>
        Hello {name}, you are {age} years old
      </p>
    </div>
  )
}
```

## Do not render objects
Objects are not valid as react children! The following code will not render and will error out:
```jsx
const App = () => {
  const friends = [
    { name: 'Peter', age: 4 },
    { name: 'Maya', age: 10 },
  ]

  return (
    <div>
      <p>{friends[0]}</p>
      <p>{friends[1]}</p>
    </div>
  )
}

export default App
```

In React, the individual things rendered in braces must be primitive values, or expressions that evaluate to primitives. The above code is trying to render objects within `<p>` tags. The fix is to use `{friends[0].name} {friends[0]}.age` instead.

Also note that React does allow arrays to be rendered, IF the contents of the array are all primitives and thus eligible for rendering.