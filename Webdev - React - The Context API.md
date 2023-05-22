#frontend #frontend-libs #webdevSpecific #react

Much of this note is taken from the official React docs, a much better source of information than the React course on free code camp which was my primary source until I realised how out of date it is. The docs are found [here](https://react.dev/learn/passing-data-deeply-with-context). 

For a look at a project that was made to help learn about context, see: [[Project Notes - Budget App]].

Managing state is a crucial part of most React applications, and as discussed in other notes in this repo, is often done by passing props between components, from parent component to child. 

However it quickly gets annoying and verbose when you need to pass props through many intermediary components, or if many components in your system need access to the same data. Context lets the parent component make information available to ANY components in the tree below it, without explicitly passing it as props through all of the intermediary child components.

[Passing props](https://react.dev/learn/passing-props-to-a-component) is a great way to explicitly pipe data through your UI tree to the components that use it. But passing props can become verbose and inconvenient when you need to pass some prop deeply through the tree, or if many components need the same prop. The nearest common ancestor could be far removed from the components that need data, and [lifting state up](https://react.dev/learn/sharing-state-between-components) that high can lead to a situation called “prop drilling”. 

![](https://react.dev/_next/image?url=%2Fimages%2Fdocs%2Fdiagrams%2Fpassing_data_prop_drilling.dark.png&w=640&q=75)
<p style="font-style: italic; text-align: center; margin: auto;">Prop Drilling</p>

The context API in React is a simple way of teleporting data to the components that need it, without explicitly passing props down long chains of components.

## Creating context and consuming context
In order to make use of context we must create and export a context using the createContext() builtin function, and that context can then be consumed using the useContext() react hook.

The three steps we need to follow are: 
1. Create a context
2. Use that context from the component that needs that data
3. Provide that context from the component that specifies that data

1. The `createContext()` function takes 0 or one args, if an arg is provided it will be the default value, the significance of that will become clear later. You should create the context in it's own JSX file and import it into whichever components need it.

2. Import the context, and the `useContext()` hook. Then call the hook on the context and assign that to a variable.

3. Add a context provider around any children that need it.

### Creating context
The create context function creates an object. The context object itself does not actually hold any information but rather represents which context other components read or provide. **This sentence is from the react docs and I still don't fully understand it**.

The context object has to properties, .Provider and .Consumer, but .Consumer is rarely used. The .Provider property is used to provide the context value to components.

### Providing Context - SomeContext.Provider
Wrap your components into a context provider to specify the value of this context for all components inside:

```jsx
function App() {
  const [theme, setTheme] = useState('light');
  // ...
  return (
    <ThemeContext.Provider value={theme}>
      <Page />
    </ThemeContext.Provider>
  );
}
```
The Provider takes a value as props. The value that you want to pass to all the components reading this context inside this provider, no matter how deep. The context value can be of any type. A component calling useContext(SomeContext) inside of the provider receives the value of the innermost corresponding context provider above it.

Context is also often used with the useReducer hook, to allow the global state to be changed through the context API in different ways depending on the payload of the action sent to the reducer.