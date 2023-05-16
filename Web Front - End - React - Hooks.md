#frontend #frontend-libs #webdevSpecific #react 

React Hooks are simple JavaScript functions that we can use to isolate the reusable part from a functional component. Hooks can be stateful and can manage side-effects.

A quick example that shows why using hooks to manipulate state or props can be much simpler than writing class components is below. The two components achieve the same thing, but the first uses a class component, and the second uses the useState hook:

```jsx
class oldButton extends React.component {
	constructor (props) {
		super(props);
		this.state = {
			count: 0
		};
	}

	render() {
		return(
			<p>{this.state.count}</p>
		);
	}
}
```

```jsx
function newButton {
	const [state] = useState({count: 0});
	return <p>{state.count}</p>
}
```

## Stateful/stateless components
Components in React can be stateful or stateless.
-   A stateful component declares and manages local state in it.
-   A stateless component is a pure function that doesn't have a local state and side-effects to manage.

A [pure function](https://blog.greenroots.info/what-are-pure-functions-and-side-effects-in-javascript) is a function without any side-effects. This means that a function always returns the same output for the same input.

If we take out the stateful and side-effects logic from a functional component, we have a stateless component. Also, the stateful and side-effects logic can be reusable elsewhere in the app. So it makes sense to isolate them from a component as much as possible.

## React hooks and stateful logic
With React Hooks, we can isolate stateful logic and side-effects from a functional component. Hooks are JavaScript functions that manage the state's behaviour and side effects by isolating them from a component.

So, we can now isolate all the stateful logic in hooks and use (compose them, as hooks are functions, too) into the components.

![image-16](https://www.freecodecamp.org/news/content/images/2022/03/image-16.png)
The question is, what is this stateful logic? It can be anything that needs to declare and manage a state variable locally.

For example, the logic to fetch data and manage the data in a local variable is stateful. We may also want to reuse the fetching logic in multiple components.

![image-17](https://www.freecodecamp.org/news/content/images/2022/03/image-17.png)

## Built in hooks
React provides a bunch of standard in-built hooks:

-   `useState`: To manage states. Returns a stateful value and an updater function to update it. See [[Web Front-End - React - The useState Hook]]
-   `useEffect`: To manage side-effects like API calls, subscriptions, timers, mutations, and more.
-   `useContext`: To return the current value for a context. See [[Web Front-End - React - The Context API]]
-   `useReducer`: A `useState` alternative to help with complex state management.
-   `useCallback`: It returns a memorized version of a callback to help a child component not re-render unnecessarily.
-   `useMemo`: It returns a memoized value that helps in performance optimizations.
-   `useRef`: It returns a ref object with a `.current` property. The ref object is mutable. It is mainly used to access a child component imperatively.
-   `useLayoutEffect`: It fires at the end of all DOM mutations. It's best to use `useEffect` as much as possible over this one as the `useLayoutEffect` fires synchronously.
-   `useDebugValue`: Helps to display a label in React DevTools for custom hooks.

You can read about these hooks in more detail [from here](https://reactjs.org/docs/hooks-reference.html). Please notice that each of these hook names start with `use`. Yes, this is a standard practice to identify a hook in the React codebase quickly.

