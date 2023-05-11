#frontend #frontend-libs #webdevSpecific #react

React is a popular JavaScript library for building reusable, component-driven user interfaces for web pages or applications. It is an Open Source view library created and maintained by Facebook, and it's a great tool to render the User Interface (UI) of modern web applications.

## JSX
React uses a syntax extension of JS called JSX which allows you to write html directly in you JavaScript. This has several benefits. It lets you use the full programmatic power of JavaScript within HTML, and helps to keep your code readable.

Because JSX is a syntactic extension of JavaScript, you can actually write JavaScript directly within JSX. To do this, you simply include the code you want to be treated as JavaScript within curly braces: `{ 'this is treated as JavaScript code' }`.

However, because JSX is not valid JavaScript, JSX code must be compiled into JavaScript. The transpiler Babel is a popular tool for this process.

A very basic example of JSX would be assigning a `<p>` tag to a variable:
```jsx
const text = <p>This text will be manipulated later on</p>;
```

When working with more complex, nested html in JSX, it must be remembered that JSX needs to return a single element. For instance, several JSX elements written as siblings with no parent wrapper element will not transpile.

Here's an example:

**Valid JSX:**
```jsx
<div>
  <p>Paragraph One</p>
  <p>Paragraph Two</p>
  <p>Paragraph Three</p>
</div>
```
**Invalid JSX:**
```jsx
<p>Paragraph One</p>
<p>Paragraph Two</p>
<p>Paragraph Three</p>
```

Comments in JSX use the syntax `{/**/}`, which is annoying.

### Render elements to the DOM with JSX
So far, you've learned that JSX is a convenient tool to write readable HTML within JavaScript. With React, we can render this JSX directly to the HTML DOM using React's rendering API known as ReactDOM.

ReactDOM offers a simple method to render React elements to the DOM which looks like this: `ReactDOM.render(componentToRender, targetNode)`, where the first argument is the React element or component that you want to render, and the second argument is the DOM node that you want to render the component to.

As you would expect, `ReactDOM.render()` must be called after the JSX element declarations, just like how you must declare variables before using them. You can pass the variable name for a react component as the first argument, and use `document.getElementById()` to target the desired html elements DOM node for the second argument:
```jsx
const JSX = (
	<div>
		<h1>Hello World</h1>
		<p>Lets render this to the DOM</p>
	</div>
);

ReactDOM.render(JSX, document.getElementById("challenge-node"));
```
This `ReactDOM.render()` call will render the html in the JSX variable within the div with`id="challenge-node"`.

### Classes in JSX and camelCase
Because class is a reserved word in JavaScript, you cannot use it to define element classes in JSX html. Instead you must use the `className` keyword.

In fact, the naming convention for all HTML attributes and event references in JSX become camelCase. For example, a click event in JSX is `onClick`, instead of `onclick`. Likewise, `onchange` becomes `onChange`. While this is a subtle difference, it is an important one to keep in mind moving forward.

### Self closing tags in JSX
In HTML, almost all tags have both an opening and closing tag: `<div></div>`; the closing tag always has a forward slash before the tag name that you are closing. However, there are special instances in HTML called “self-closing tags”, or tags that don’t require both an opening and closing tag before another tag can start.

For example the line-break tag can be written as `<br>` or as `<br />`, but should never be written as `<br></br>`, since it doesn't contain any content.

In JSX, the rules are a little different. Any JSX element can be written with a self-closing tag, and every element must be closed. The line-break tag, for example, must always be written as `<br />` in order to be valid JSX that can be transpiled. A `<div>`, on the other hand, can be written as `<div />` or `<div></div>`. The difference is that in the first syntax version there is no way to include anything in the `<div />`. You will see in later challenges that this syntax is useful when rendering React components.

## What's next
This has been a basic overview of JSX, the foundational topic of react. The following notes continue down the React rabbit hole, starting with components, the core of React:
	[[Web Front-End - React - Components]]https://open.spotify.com/track/3ZvEl3WmYzo9oYViKOzVeS?si=ac899045c8744ad8