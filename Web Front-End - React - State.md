#frontend #frontend-libs #webdevSpecific #react 

One of the most important topics in React is `state`. State consists of any data your application needs to know about, that can change over time. You want your apps to respond to state changes and present an updated UI when necessary. React offers a nice solution for the state management of modern web applications.

You create state in a React component by declaring a `state` property on the component class in its `constructor`. This initializes the component with `state` when it is created. The `state` property must be set to a JavaScript `object`. Declaring it looks like this:
```jsx
this.state = {
}
```
You have access to the `state` object throughout the life of your component. You can update it, render it in your UI, and pass it as props to child components. The `state` object can be as complex or as simple as you need it to be. Note that you must create a class component by extending `React.Component` in order to create `state` like this.

## Render state in the UI
Once you define a component's initial state, you can display any part of it in the UI that is rendered. If a component is stateful, it will always have access to the data in `state` in its `render()` method. You can access the data with `this.state`.

If you want to access a state value within the `return` of the render method, you have to enclose the value in curly braces.

`state` is one of the most powerful features of components in React. It allows you to track important data in your app and render a UI in response to changes in this data. If your data changes, your UI will change. React uses what is called a virtual DOM, to keep track of changes behind the scenes. When state data updates, it triggers a re-render of the components using that data - including child components that received the data as a prop. React updates the actual DOM, but only where necessary. This means you don't have to worry about changing the DOM. You simply declare what the UI should look like.

<blockquote style="color: cyan">Note that if you make a component stateful, no other components are aware of its `state`. Its `state` is completely encapsulated, or local to that component, unless you pass state data to a child component as `props`. This notion of encapsulated `state` is very important because it allows you to write certain logic, then have that logic contained and isolated in one place in your code.</blockquote>

There is another way to access `state` in a component. In the `render()` method, before the `return` statement, you can write JavaScript directly. For example, you could declare functions, access data from `state` or `props`, perform computations on this data, and so on. Then, you can assign any data to variables, which you have access to in the `return` statement. See the comments in the below snippet for an example:
```jsx
class MyComponent extends React.Component {
	constructor(props) {
		super(props);
		this.state = {
			name: 'freeCodeCamp'
		}
	}
	render() {
		// You can write JS here without braces - note the js comment syntax
		const name = this.state.name;
		return (
			<div>
				{ /* The return is jsx; you need braces for the JS variable */ }
				<h1>{name}</h1>
			</div>
		);
	}
};
```

## Set the state with this.setState()
Previously we covered component `state` and how to initialize state in the `constructor`. There is also a way to change the component's `state` after it has been set. React provides a method for updating component `state` called `setState`. You call the `setState` method within your component class like so: `this.setState()`, passing in an object with key-value pairs. The keys are your state properties and the values are the updated state data. For instance, if we were storing a `username` in state and wanted to update it, it would look like this:
```jsx
this.setState({
  username: 'Lewis'
});
```
React expects you to never modify `state` directly, instead always use `this.setState()` when state changes occur. Also, you should note that React may batch multiple state updates in order to improve performance. What this means is that state updates through the `setState` method can be asynchronous. There is an alternative syntax for the `setState` method which provides a way around this problem. This is rarely needed but it's good to keep it in mind! Consult the free code camp [React article](https://www.freecodecamp.org/news/what-is-state-in-react-explained-with-examples/) for further details.

## Bind 'this' to a class method
In addition to setting and updating `state`, you can also define methods for your component class. A class method typically needs to use the `this` keyword so it can access properties on the class (such as `state` and `props`) inside the scope of the method. There are a few ways to allow your class methods to access `this`.

One common way is to explicitly bind `this` in the constructor so `this` becomes bound to the class methods when the component is initialized. For example you would use something like: `this.handleClick = this.handleClick.bind(this)` in your definition of a `handleClick` method, in the constructor. Then, when you call a function like `this.setState()` within the class method, `this` refers to the class rather than the method scope and will not be `undefined`.

NOTE: The this keyword is a confusing topic in JS, see the [mozilla docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) for a more thorough explaination.

