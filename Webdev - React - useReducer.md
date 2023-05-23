#frontend #frontend-libs #webdevSpecific #react 

As applications grow and get larger, the state tree gets more and more complicated. With only a few pieces of state referenced in top level components, it's fairly easy to see how it all fits together, but it gets very confusing if we have hundereds of state variables to manage. A pattern that has emerged to address this is reducer functions and the useReducer hook.

At it's core a reducer function is just a pure function that decides how to update the state based on an action, usually using a switch case statement. 

## Actions
An action is a vanilla JS object that describes a change to the state properties. An action doesn't do anything on it's own, it is just instructions. By convention, action objects must have a type property, and its value must be in all caps. An example action would be something like this:
```jsx
const myAction = {
	type: "TOGGLE_THEME",
	payload:  "light"
};
```

## Reducer
As mentioned above, the reducer is just a pure JS function that uses a reducer to update state in different ways based on the type of action it is passed as an arg. For example:
```jsx
const myReducer = (state, action) => {
	switch(action.type) {
		case "TOGGLE_THEME":
			const newTheme = action.payload === "light" ?
				"dark" : "light"
			return {
				...state,
				theme: newTheme
			};
		default:
			return state;
	}
}
```
The most important thing about reducer functions is that they are **PURE**. Meaning that they never change the values of the args provided to them, and that a given input will always return the same output. You can see in the above example that when there is no match to a case, it will return the original state, and when there is a match, it **COPIES** the original state into a new object, and overwrites the part of state that needs to be updated by the action passed to it.

## useReducer
This is the hook that actually enables the use this pattern, where changes to state can be different in response to different actions as laid out in a reducer function. When you call the useReducer hook you must provide the reducer function as the first argument, and the initial state as the second arg.

When you call this hook, you are returned the current state and a dispatch function, The dispatch function is used to send an action to the reducer and change the state value. Note that useReducer is very similar to the useState hook, the difference being that useReducer gives us a state from outside the current component and a setter for that state, whereas useState gives us a local state and setter that is scoped to the current component and must be passed as props to be used in child components, down a chain.

## dispatch
When we use the `useReducer` hook, the second item we are given is a function called `dispatch`. The sole responsibility of dispatch is to take Actions, and send them to the Reducer. Dispatch is smart, so it will call the Reducer for us, with the correct state. All we need to do is pass the Action in.