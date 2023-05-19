#frontend #frontend-libs #webdevSpecific #react 

The useState hook lets us manipulate state in a functional component, and it fills a similar use case as `this.setState()` does in class components.

Before you can use the hook you must import it:
```jsx
import { useState } from "react";
```
Notice that we are destructuring `useState` from `react` as it is a named export.

We initialize our state by calling `useState` in our function component. `useState` accepts an initial state and returns two values:
-   The current state.
-   A function that updates the state.
```jsx
function FavoriteColor() {
  const [color, setColor] = useState("");
}
```
The above code sets the state object as: `{color: ""}` and returns the `setColor()` function to update it.