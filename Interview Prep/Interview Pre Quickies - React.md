#interviewprep #revision 

## State
When working with reactive front ends, you need a way to store and keep track of what the user has done, so that if the user has clicked on a checkbox or written test in a form field, you have a record of that and can make the UI respond. The UI is bound to the state and components are conditionally rendered or mutated to reflect the current state, which is informed by the users actions.

Modern react is functional react, so we aren't storing state in class components like we used to. You initialize a state with the `useState()` hook, and get back the state variable set to your provided default, and a setter function to change it. Whenever state changes, the component that it belongs to will re-render. The state setter function takes in a new state, compares it to the old one, merges them and then triggers the re-render. You should never mutate the state value directly without involving the state setter function.

[Here](https://stackoverflow.com/questions/37755997/why-cant-i-directly-modify-a-components-state-really) is a great stack overflow answer that covers some of the nuances of this topic.

## React lifecycle, methods, hooks
The three phases of a React components life cycle are:
1. Mounting
2. Updating
3. Unmounting
Mounting is when the new component is created and inserted into the DOM. This is the start of it's life and only happens once. It is also called the initial render of the component. Updating is mutation or updating of the component, and unmounting is the removal of the component from the DOM. A react component may not always go through every part of this life cycle, as they do not always need to be updated and may not be unmounted.

The below diagram illustrates the life-cycle of a modern react component, including the lifecycle methods that run under the hood to confirm/control the mounting/unmounting and updating of components:
![[Pasted image 20230818144147.png]]

[Here](https://medium.com/codex/the-lifecycle-of-a-react-component-8e01332a068d)is a good article on the details of comonent life cycle and the lifecycle methods.

## The react virtual DOM vs the real DOM
[This](https://medium.com/devinder/react-virtual-dom-vs-real-dom-23749ff7adc9)is a useful article for reference.

The DOM is the document object model. It describes the structure and content of a document (ie the HTML), and provides a way to access and manipulate the document. When a browser loads an HTML doc, it processes the markdown into a tree in which every node is an object representing an element of the document that can be manipulated to change the structure and content. 

The rub is that when any element in the DOM is changed, the entire DOM is re-rendered at once, which is inefficient and puts more load on the server than is strictly necessary. The **virtual DOM** is a technology used by React to improve performance for reactive web views. <span style="color: cyan; font-weight: bold; font-style: italic;">The virtual DOM is a lightweight copy of the DOM for a web page that is created in memory.</span> When the component state changes, the virtual DOM is recalculated, compared to the previous iteration, and only the changed elements are re-rendered.