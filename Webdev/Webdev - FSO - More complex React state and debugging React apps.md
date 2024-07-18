#webdevSpecific 

In the previous examples used in the FSO react content so far, the application state was very simple and comprised only a single integer (the counter value). 

In most cases the application will require more complex state, and the best way to handle this is usually by using the `useState` hook multiple times to create seperate pieces of independant state. 

In the below we create two states for the `App` component:
```jsx
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)

  return (
    <div>
      {left}
      <button onClick={() => setLeft(left + 1)}>
        left
      </button>
      <button onClick={() => setRight(right + 1)}>
        right
      </button>
      {right}
    </div>
  )
}
```

Because state values can be of any type, we could refactor this to have both the left and right states in one state value, as an object:
```jsx
const App = () => {
  const [clicks, setClicks] = useState({
    left: 0, right: 0
  })

const handleLeftClick = () =>
  setClicks({ ...clicks, left: clicks.left + 1 })

const handleRightClick = () =>
  setClicks({ ...clicks, right: clicks.right + 1 })

  return (
    <div>
      {clicks.left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {clicks.right}
    </div>
  )
}
```
Now the component only has one piece of state, and the event handlers have to update the entire state. Note also the use of the spread operator in the event handler definitions.

You might think to increment the state and then update it like this:
```js
const handleLeftClick = () => {
  clicks.left++
  setClicks(clicks)
}
```
BUT YOU SHOULD NOT DO THIS!
The application will seem to work but _it is forbidden in React to mutate state directly_, since [it can result in unexpected side effects](https://stackoverflow.com/a/40309023). <span style="color: cyan; font-weight: bold">Changing state has to always be done by setting the state to a new object.</span> If properties from the previous state object are not changed, they need to simply be copied, which is done by copying those properties into a new object and setting that as the new state.

Storing all of the state in a single state object is a bad choice for this particular application; there's no apparent benefit and the resulting application is a lot more complex. 

There are situations where it can be beneficial to store a piece of application state in a more complex data structure. [The official React documentation](https://react.dev/learn/choosing-the-state-structure) contains some helpful guidance on the topic.

## Handling arrays
Lets update the example to include a third piece of state that tracks a list of every Left or Right click made:
```jsx
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
  const [allClicks, setAll] = useState([])

  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
  }

  const handleRightClick = () => {
    setAll(allClicks.concat('R'))
    setRight(right + 1)
  }

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <p>{allClicks.join(' ')}</p>
    </div>
  )
}
```
Every time a left or right click is made, an L or R is concatenated onto the list in state. Note that the concat method does not mutate the existing array, it returns a copy with the new item added. You COULD use the push method, but SHOULD NOT. <span style="color: cyan; font-weight: bold;">Remember that you are not supposed to directly mutate stateful values in React.</span>

## Updates to state are asynchronous
Calls to setter functions for `useState` invocations are execute asynchronously under the hood by React, meaning that multiple state updates will be batched together and executed at a time that makes sense before the re-render is completed. 

What this means in practice is that you have to be careful when one state is to be updated based on another. 

Look at this example:
```jsx
const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
  const [allClicks, setAll] = useState([])

  const [total, setTotal] = useState(0)

  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
    setTotal(left + right)
  }

  const handleRightClick = () => {
    setAll(allClicks.concat('R'))
    setRight(right + 1)
    setTotal(left + right)
  }

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <p>{allClicks.join(' ')}</p>
      <p>total {total}</p>
    </div>
  )
}
```
This code will not quite work properly, with the total count of clicks always lagging one behind the real total. The issue, is in the event handlers, where state for the left or right click is updated, and then the total click state is updated based on the current state values. <span style="color: cyan; font-weight: bold">We cannot guarantee that the `setLeft` or `setRight` call will actually run before the `setTotal` call, because of the async, batched nature of React state updates.</span>
 We can fix this by assigning the new value for the left or right state to a variable, and using this variable to update the Total state, guaranteeing it gets the correct value and is not dependant on other states updating in time.
```jsx
const App = () => {
  // ...
  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    const updatedLeft = left + 1
    setLeft(updatedLeft)
    setTotal(updatedLeft + right) 
  }

  // ...
}
```

## Conditional rendering
Let's modify our application so that the rendering of the clicking history is handled by a new _History_ component:
```jsx
const History = (props) => {
  if (props.allClicks.length === 0) {
    return (
      <div>
        the app is used by pressing the buttons
      </div>
    )
  }
  return (
    <div>
      button press history: {props.allClicks.join(' ')}
    </div>
  )
}

const App = () => {
  // ...

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}

      <History allClicks={allClicks} />
    </div>
  )
}
```

Now the behavior of the component depends on whether or not any buttons have been clicked. If not, meaning that the _allClicks_ array is empty, the component renders a div element with some instructions instead:
```jsx
<div>the app is used by pressing the buttons</div>
```

And in all other cases, the component renders the clicking history:
```jsx
<div>
  button press history: {props.allClicks.join(' ')}
</div>
```

The _History_ component renders completely different React elements depending on the state of the application. This is called _conditional rendering_.

React also offers many other ways of doing [conditional rendering](https://react.dev/learn/conditional-rendering). We will take a closer look at this in [part 2](https://fullstackopen.com/en/part2).

## Debugging react apps
A good place to start is the traditional `console.log()` peppering. Logging the props object of a component or the data that it is supposed to fetch is a good example. **NB** When you use _console.log_ for debugging, don't combine _objects_ in a Java-like fashion by using the plus operator:
```jsx
console.log('props value is ' + props)
```

If you do that, you will end up with a rather uninformative log message:
```jsx
props value is [object Object]
```

Instead, separate the things you want to log to the console with a comma:
```jsx
console.log('props value is', props)
```

You can also use the debug tab of the browser dev tools to add breakpoints to the code by typing `debugger` in the source code. This will pause execution at that point. 

Adding the react dev tools browser extension will also let you view the state values for components in the page from the devtools.

## Rules of hooks
There are a few limitations and [rules](https://react.dev/warnings/invalid-hook-call-warning#breaking-rules-of-hooks) that we have to follow to ensure that our application uses hooks-based state functions correctly.

The _useState_ function (as well as the _useEffect_ function introduced later on in the course) _must not be called_ from inside of a loop, a conditional expression, or any place that is not a function defining a component. This must be done to ensure that the hooks are always called in the same order, and if this isn't the case the application will behave erratically.

To recap, hooks may only be called from the inside of a function body that defines a React component:
```js
const App = () => {
  // these are ok
  const [age, setAge] = useState(0)
  const [name, setName] = useState('Juha Tauriainen')

  if ( age > 10 ) {
    // this does not work!
    const [foobar, setFoobar] = useState(null)
  }

  for ( let i = 0; i < age; i++ ) {
    // also this is not good
    const [rightWay, setRightWay] = useState(false)
  }

  const notGood = () => {
    // and this is also illegal
    const [x, setX] = useState(-1000)
  }

  return (
    //...
  )
}
```

## Passing handlers as props
Let's extract the button into its own component:
```js
const Button = (props) => (
  <button onClick={props.handleClick}>
    {props.text}
  </button>
)
```
The component gets the event handler function from the _handleClick_ prop, and the text of the button from the _text_ prop.

## Don't define components inside other components
This is possible to do and can be done without immediately breaking the app, but you still should not do it. The method provides no benefits and leads to many unpleasant problems. The biggest problems are because React treats a component defined inside of another component as a new component in every render. This makes it impossible for React to optimize the component.