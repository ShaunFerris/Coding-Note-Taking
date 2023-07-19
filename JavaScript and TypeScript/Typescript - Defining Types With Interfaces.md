#typescript 

You can use TS to type the built-in object like string and number, but what if you need to define the shape of an object that you are implementing yourself?

JavaScript supports a large number of programming paradigms and patterns, including some like dynamic programming patterns that make infering types quite difficult for the TypeScript engine. To cover such cases, TS gives you the ability to define interfaces, which let you tell typescript what types to expect from an object.

There is already a small set of primitive types available in JavaScript: `boolean`, `bigint`, `null`, `number`, `string`, `symbol`, and `undefined`, which you can use in an interface. TypeScript extends this list with a few more, such as `any` (allow anything), [`unknown`](https://www.typescriptlang.org/play#example/unknown-and-never) (ensure someone using this type declares what the type is), [`never`](https://www.typescriptlang.org/play#example/unknown-and-never) (it’s not possible that this type could happen), and `void` (a function which returns `undefined` or has no return value).

You’ll see that there are two syntaxes for building types: [Interfaces and Types](https://www.typescriptlang.org/play/?e=83#example/types-vs-interfaces). You should prefer `interface`. Use `type` when you need specific features.

## Basic interface example
As an example, you could define an object like below and let TS infer the types automatically:
```typescript
const user = {
	name: "Shaun",
	id: 0
};
```
From this definition, TS will infer `name: string, id: number`. You could also explicitly describe the shape of the user object by invoking an interface and then declare that an object should conform to the shape of that interface with the following syntax:
```typescript
interface User {
	name: string;
	id: number;
}

//name: Interface name tells typescript the shape of the object
const user: User = {
	name: "Shaun",
	id: 0
};
```

If you were to define an object and type it with the interface like above, but the shape does not match, then TS will give you a warning in the editor and fail to compile.

## Using interface declarations with classes
Because JS supports OOP patterns, so does TS and you can use interface declarations with class definitions like this:
```typescript
interface User {
	name: string;
	id: number;
}

class UserAccount {
	name: string;
	id: number;

	constructor(name: string, id: number) {
		this.name = name;
		this.id = id;
	}
}

const user: User = new UserAccount("Shaun", 0)
```

## Interfaces with functions
You can use interfaces to annotate parameters and return values to functions:
```typescript
//Argument should fit the user interface type
function deleteUser(user: User) {
	...
}

//Return should fit the User interface shape
function getAdmin(): User {
	...
}
```

## Interfaces and types
In addition to defining interfaces, TS lets you define types.

`Type aliases` and `interfaces` are very similar, and in many cases you can choose between them freely. Almost all features of an `interface` are available in `type`, the key distinction is that a `type` cannot be re-opened to add new properties vs an `interface` which is always extendable.

See: [[Typescript - Defining Types With Custom Types]]