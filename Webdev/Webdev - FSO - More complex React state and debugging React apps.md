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
