#typescript 

## Typing React FCs
TS provides a few interfaces for writing react components with typesafety, including `React.FC` for functional components. You could use this type on all functional components but this is not necessarily best practices, as you can just let the compiler infer the type. However it is worth knowing some quirks of both cases.

When you explicitly type a FC you need to type the props inside a generic, whereas if you let the compiler infer the type you can type the props normal. For example, with `React.FC`:
```typescript
const ExampleHeader: React.FC<{ loading: boolean }> = ({ loading }) => {
	...
	return ...
}
```
And without the explicit `React.FC` type:
```typescript
const ExampleHeader = ({ loading }: { loading: boolean }) => {
	...
	return ...
}
```
The advantage of explicitly typing a functional component is that you get access to the children type and types for inate component properties like `displayName`, but if you don't need these then it is often more readable to use the second format where you only type your custom props.

## Extending native HTML props
You can extend the props of native HTML elements with custom props using an interface like this:
```typescript
export interface ExampleProps extends React.HTMLAttributes<HTMLDivElement> {
	description: string;
	name: string;
}
```
