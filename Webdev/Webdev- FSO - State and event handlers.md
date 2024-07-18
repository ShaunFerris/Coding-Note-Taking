#webdevSpecific 

## Component helper functions
For this section we will be using a very simple example app from the fso course content:
```jsx
const Hello = (props) => {
  const bornYear = () => {    
    const yearNow = new Date().getFullYear()    
    return yearNow - props.age  
  }
  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
      <p>So you were probably born in {bornYear()}</p>    
    </div>
  )
}
```

The logic for guessing a users birth year is organized into a function that is called when the Hello component is rendered. The persons age does not need to be passed as a parameter to the `bornYear` function, as the function is within the components scope so it can access all props passed to the component. Born year is defined within the component function, which is a common pattern in JS, but less common and more difficult to do in languages like Java.

## Destructuring
This has been covered by me a lot before and I also use it all the time so minimal notes here. Simply put it is an ES6 feature that lets you assign the values of object properties directly to variables. It is very useful and commonly used for pulling what you need out of the props object and into variables to use in component logic. You could use it in the `Hello` component above like this:
```jsx
const Hello = ({name, age}) => {
  const bornYear = () => {    
    const yearNow = new Date().getFullYear()    
    return yearNow - age  
  }
  return (
    <div>
      <p>
        Hello {name}, you are {age} years old
      </p>
      <p>So you were probably born in {bornYear()}</p>    
    </div>
  )
}
```

## Page re-rendering
All the examples shown by fso so far have been such that they do not change after the initial render. What if we wanted to add interactivity, like a button that increments a counter?

Lets start with the following:
```jsx
//App.jsx
const App = (props) => {
  const {counter} = props
  return (
    <div>{counter}</div>
  )
}

export default App
```
and
```jsx
//main.jsx
import ReactDOM from 'react-dom/client'

import App from './App'

let counter = 1

ReactDOM.createRoot(document.getElementById('root')).render(
  <App counter={counter} />
)
```

The App component is given the value of the counter via the `counter` prop and renders it;s value. But if the counter value were to change, it's rendering would not, the value on the screen would not update unless we somehow trigger a re-render.

A primitive way of rendering the counter with multiple values would be some kind of abomination like this:
```jsx
let counter = 1

const refresh = () => {
  ReactDOM.createRoot(document.getElementById('root')).render(
    <App counter={counter} />
  )
}

refresh()
counter += 1
refresh()
counter += 1
refresh()
```

Now the counter value will be rendered as 1, then 2, and finally 3. However the re-renders happen so fast that you can't really see them.

You could improve this shit show slightly by using an interval to call the refresh function:
```js
setInterval(() => {
  refresh()
  counter += 1
}, 1000)
```

But, making repeated calls to the React render method is not reccommended. To handle re-rendering of components in a sane way, use state.

## Stateful components
The fso examples have so far avoided working with state, and have not contained any data that could change during the component life-cycle.

Lets change that by updating our example App component to use a stateful counter. The main and App jsx files look like this:
```jsx
//main.jsx
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

```jsx
//App.jsx
import { useState } from 'react'

const App = () => {

  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  return (
    <div>{counter}</div>
  )
}

export default App
```

The `useState` call in the App component initialises a stateful `counter` variable to 0, and uses `setTimeout` to update the state one second after the initial render, incrementing it by 1. 

When the `setTimeout` function call fires, and calls `setCounter` after one second, it triggers a re-render, because state has changed. Any time that state is updated, the component and all it's children will be re-rendered. 

When the component is re-rendered , the component function is executed agains and `useState` is called again. This time it has a value, so instead of returning the default value of 0, it returns 1. The `setTimeout` also gets called again on re-render, so you can see how this code has created a cycle of incrementing state, which causes a re-render, which causes another increment.

## Event handling
FSO content has already mentioned event handlers, which are functions registered to be called when a specific event occurs, usually linked to user interaction. 

Lets change the counter example to be triggered by a user clicking a button instead of a time out:
```jsx
const App = () => {
  const [ counter, setCounter ] = useState(0)


  const handleClick = () => {
    console.log('clicked')
  }

  return (
    <div>
      <div>{counter}</div>

      <button onClick={handleClick}>
        plus
      </button>
    </div>
  )
}
```

We set the value of the buttons `onClick` attribute to a reference to a handler function that we define in the component body. Now every click of the button will call `handleClick`, logging the message "clicked". 

The event handler could alternatively be defined directly inside the `onClick` attribute:
```jsx
const App = () => {
  const [ counter, setCounter ] = useState(0)

  return (
    <div>
      <div>{counter}</div>

      <button onClick={() => console.log('clicked')}>
        plus
      </button>
    </div>
  )
}
```

By changing the event handler to use state, we achieve the desired result of incrementing the counter on button click:
```jsx
<button onClick={() => setCounter(counter + 1)}>
  plus
</button>
```

## Event handlers are functions
We define the event handlers for our buttons where we declare their _onClick_ attributes:
```jsx
<button onClick={() => setCounter(counter + 1)}> 
  plus
</button>
```

What if we tried to define the event handlers in a simpler form?
```jsx
<button onClick={setCounter(counter + 1)}> 
  plus
</button>
```

This would break the application. This is because an event handler is supposed to be either a function or a function reference, in the above we have tried to set the handler to a function call. This breaks the app because the setCounter call is executed on render, causing infinite re-renders. Putting it in a function stops it being directly evaluated on render. 

## Passing state to child components
It's recommended to write React components that are small and reusable across the application and even across projects. Let's refactor our application so that it's composed of three smaller components, one component for displaying the counter and two components for buttons.

Let's first implement a _Display_ component that's responsible for displaying the value of the counter.

One best practice in React is to [lift the state up](https://react.dev/learn/sharing-state-between-components) in the component hierarchy. The documentation says:

> _Often, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor._

So let's place the application's state in the _App_ component and pass it down to the _Display_ component through _props_:
```jsx
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

Using the component is straightforward, as we only need to pass the state of the _counter_ to it:
```jsx
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/>      <button onClick={increaseByOne}>
        plus
      </button>
      <button onClick={setToZero}> 
        zero
      </button>
    </div>
  )
}
```

Everything still works. When the buttons are clicked and the _App_ gets re-rendered, all of its children including the _Display_ component are also re-rendered.

Next, let's make a _Button_ component for the buttons of our application. We have to pass the event handler as well as the title of the button through the component's props:
```jsx
const Button = (props) => {
  return (
    <button onClick={props.onClick}>
      {props.text}
    </button>
  )
}
```

Our _App_ component now looks like this:
```jsx
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)
  const decreaseByOne = () => setCounter(counter - 1)  
  const setToZero = () => setCounter(0)
  
  return (
    <div>
      <Display counter={counter}/>
	  <Button onClick={increaseByOne} text='plus'/>      
	  <Button onClick={setToZero} text='zero'/>           
	  <Button onClick={decreaseByOne} text='minus'/>               
    </div>
  )
}

```

Since we now have an easily reusable _Button_ component, we've also implemented new functionality into our application by adding a button that can be used to decrement the counter.

The event handler is passed to the _Button_ component through the _onClick_ prop. The name of the prop itself is not that significant, but our naming choice wasn't completely random.

React's own official [tutorial](https://react.dev/learn/tutorial-tic-tac-toe) suggests: "In React, it’s conventional to use onSomething names for props which take functions which handle events and handleSomething for the actual function definitions which handle those events."

## Changes in state cause re-renders
To re-cap what has been covered here:

When the app starts, the code in the `App` component is executed. The code uses the `useState` hook to create the app state, setting the initial counter value to 0. The component displays the `Display` component - which displays the counters value, 0, and three button components. The buttons all have event handlers, which change the state in some way. 

When one of the buttons is clicked the handler is executed, which changes the state of the App component with the `setCounter` function **causing a re-render**. 

So, if a user clicks the _plus_ button, the button's event handler changes the value of _counter_ to 1, and the _App_ component is re-rendered. This causes its subcomponents _Display_ and _Button_ to also be re-rendered. _Display_ receives the new value of the counter, 1, as props. The _Button_ components receive event handlers which can be used to change the state of the counter.

## Next
This is the end of the FSO section on state and event handlers, the next section is [[Webdev - FSO - More complex React state and debugging React apps]].