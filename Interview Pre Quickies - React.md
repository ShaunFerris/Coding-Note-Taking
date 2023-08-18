#interviewprep #revision 

## State
When working with reactive front ends, you need a way to store and keep track of what the user has done, so that if the user has clicked on a checkbox or written test in a form field, you have a record of that and can make the UI respond. The UI is bound to the state and components are conditionally rendered or mutated to reflect the current state, which is informed by the users actions.

Modern react is functional react, so we aren't storing state in class components like we used to. You initialize a state with the `useState()` hook, and get back the state variable set to your provided default, and a setter function to change it. Whenever state changes, the component that it belongs to will re-render. The state setter function takes in a new state, compares it to the old one, merges them and then triggers the re-render. You should never mutate the state value directly without involving the state setter function.

[Here](https://stackoverflow.com/questions/37755997/why-cant-i-directly-modify-a-components-state-really) is a great stack overflow answer that covers some of the nuances of this topic.

## React lifecycle, methods, hooks
