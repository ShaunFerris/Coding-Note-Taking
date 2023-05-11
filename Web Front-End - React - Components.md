#frontend #frontend-libs #webdevSpecific #react 

Components are the core of React. Everything in React is a component and here you will learn how to create one.

## Defining components
There are two ways to create a React component. The first way is to use a JavaScript function.

### Defining components with function syntax
**Defining a component in this way creates a _stateless functional component_.** The concept of state in an application will be covered in later challenges. For now, think of a stateless component as one that can receive data and render it, but does not manage or track changes to that data. (We'll cover the second way to create a React component in the next section.)

To create a component with a function, you simply write a JavaScript function that returns either JSX or `null`. One important thing to note is that React requires your function name to begin with a capital letter. Here's an example of a stateless functional component that assigns an HTML class in JSX:
```jsx
const DemoComponent = function() {
  return (
    <div className='customClass' />
  );
};
```
After being transpiled, the `<div>` will have a CSS class of `customClass`.

Because a JSX component represents HTML, you could put several components together to create a more complex HTML page. This is one of the key advantages of the component architecture React provides. It allows you to compose your UI from many separate, isolated components. This makes it easier to build and maintain complex user interfaces.

### Defining components with class syntax
The other way to define a React component is with the ES6 `class` syntax. This can create components that, unlike the previous example, are not stateless.

In the following example, `Kitten` extends `React.Component`:
```jsx
class Kitten extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <h1>Hi</h1>
    );
  }
}
```
This creates an ES6 class `Kitten` which extends the `React.Component` class. So the `Kitten` class now has access to many useful React features, such as local state and lifecycle hooks. Don't worry if you aren't familiar with these terms yet, they will be covered in greater detail in later notes. Also notice the `Kitten` class has a `constructor` defined within it that calls `super()`. It uses `super()` to call the constructor of the parent class, in this case `React.Component`. The constructor is a special method used during the initialization of objects that are created with the `class` keyword. It is best practice to call a component's `constructor` with `super`, and pass `props` to both. This makes sure the component is initialized properly. For now, know that it is standard for this code to be included. Soon you will see other uses for the constructor as well as `props`.

### Defining components with composition
Now we will look at how we can compose multiple React components together. Imagine you are building an app and have created three components: a `Navbar`, `Dashboard`, and `Footer`.

To compose these components together, you could create an `App` _parent_ component which renders each of these three components as _children_. To render a component as a child in a React component, you include the component name written as a custom HTML tag in the JSX. For example, in the `render` method you could write:
```jsx
return (
 <App>
  <Navbar />
  <Dashboard />
  <Footer />
 </App>
)
```
When React encounters a custom HTML tag that references another component (a component name wrapped in `< />` like in this example), it renders the markup for that component in the location of the tag. This should illustrate the parent/child relationship between the `App` component and the `Navbar`, `Dashboard`, and `Footer`.

Component composition is one of React's powerful features. When you work with React, it is important to start thinking about your user interface in terms of components like the App example in the last challenge. You break down your UI into its basic building blocks, and those pieces become the components. This helps to separate the code responsible for the UI from the code responsible for handling your application logic. It can greatly simplify the development and maintenance of complex projects.

As we continue to use more complex compositions with React components and JSX, there is one important point to note:

<blockquote style="color: cyan">Rendering ES6 style class components within other components is no different than rendering the simple components you used in the last few challenges. You can render JSX elements, stateless functional components, and ES6 class components within other components.</blockquote>

