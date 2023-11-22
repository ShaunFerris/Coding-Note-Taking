#webdevSpecific #react #hooks

## What is the useRef hook
It allows you to reference a value that is not needed for rendering the view. The reference is initialized with an initial value. Basically it works like useState, in that it stores a variable that is not reinitialized on render of the component, but updating the value of the ref does not cuase a re-render like state value updates do.

Declare a ref at the top level of a component like this:
```jsx
import { useRef } from 'react';

function myComponent() {
	const intervalRef = useRef(0); 
	const inputRef = useRef(null);
}
```

The hook returns an object with a single property `{ current: 0 }`. Current is initially set to the initial value, and can later be set to something else. If you pass the Ref object to React as `ref` attribute on a JSX node, React will set it's `current` property. On the next re-render useRef will return the updated object.
## Usage and differences from useState
`useRef` returns a ref object with a single `current` property initially set to the initial value you provided.

On the next renders, `useRef` will return the same object. You can change its `current` property to store information and read it later. This might remind you of [state](https://react.dev/reference/react/useState), but there is an important difference.

**Changing a ref does not trigger a re-render.** This means refs are perfect for storing information that doesn’t affect the visual output of your component. For example, if you need to store an [interval ID](https://developer.mozilla.org/en-US/docs/Web/API/setInterval) and retrieve it later, you can put it in a ref. To update the value inside the ref, you need to manually change its `current` property:
```jsx
function handleStartClick() {
  const intervalId = setInterval(() => {
    // ...
  }, 1000);
  intervalRef.current = intervalId;
}
```

Later, you can read that interval ID from the ref so that you can call [clear that interval](https://developer.mozilla.org/en-US/docs/Web/API/clearInterval):
```jsx
function handleStopClick() {
  const intervalId = intervalRef.current;
  clearInterval(intervalId);
}
```

By using a ref, you ensure that:
- You can **store information** between re-renders (unlike regular variables, which reset on every render).
- Changing it **does not trigger a re-render** (unlike state variables, which trigger a re-render).
- The **information is local** to each copy of your component (unlike the variables outside, which are shared).

Changing a ref does not trigger a re-render, so refs are not appropriate for storing information you want to display on the screen. Use state for that instead. Read more about [choosing between `useRef` and `useState`.](https://react.dev/learn/referencing-values-with-refs#differences-between-refs-and-state)