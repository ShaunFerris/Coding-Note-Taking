#frontend #frontend-libs #webdevSpecific #react 

This note is ripped pretty much verbatim from the docs, for my own reference offline.

useMemo is a hook that allows you to cache a value or the result of a calculation of some value, in between renders. It is useful mostly in performance optimizations as it allows you to avoid recalculating the same thing every time a component is re-rendered.

```jsx
const cachedValue = useMemo(valueCalcFunction, dependancies: [])
```

#### Parameters[](https://react.dev/reference/react/useMemo#parameters "Link for Parameters")
- `valueCalcFunction`: The function calculating the value that you want to cache. It should be pure, should take no arguments, and should return a value of any type. React will call your function during the initial render. On next renders, React will return the same value again if the `dependencies` have not changed since the last render. Otherwise, it will call `calculateValue`, return its result, and store it so it can be reused later.

- `dependencies`: The list of all reactive values referenced inside of the `calculateValue` code. Reactive values include props, state, and all the variables and functions declared directly inside your component body. If your linter is [configured for React](https://react.dev/learn/editor-setup#linting), it will verify that every reactive value is correctly specified as a dependency. The list of dependencies must have a constant number of items and be written inline like `[dep1, dep2, dep3]`. React will compare each dependency with its previous value using the [`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) comparison.

#### Returns[](https://react.dev/reference/react/useMemo#returns "Link for Returns")
On the initial render, `useMemo` returns the result of calling `calculateValue` with no arguments.

During next renders, it will either return an already stored value from the last render (if the dependencies haven’t changed), or call `calculateValue` again, and return the result that `calculateValue` has returned.