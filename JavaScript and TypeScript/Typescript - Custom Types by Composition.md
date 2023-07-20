#typescript 

TypeScript allows you to create complex types that are combinations of simple types by composition. The two main ways to do this are with <span style="color: cyan;">unions</span> and <span style="color: cyan;">generics</span>.

## Unions
Unions allow you to define a type as one of many possible types, or as many possible values withing a type. You can define union types using the actual values. For example you could manually define a boolean type like this:
```typescript
type MyBool = true | false //the pipe is the union operator
```
If you hover over the MyBool variable in an IDE it will show up as a boolean. This is a feature of the structural type system, discussed here: [[Typescript - The Structural Type System]].

A common usecase for this kind of type definition is to define a specific set of literal values that a variable is allowed to be in order to be considered a member of the type:
```typescript
type WindowStates = "open" | "closed" | "minimized";
type LockStates = "locked" | "unlocked";
type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 9;
```

As well as unions of values within a type like above, they can be used to define a type that can be one of multiple types. I.e: a type that is either an array OR a string:
```typescript
function getLength(obj: string | string[]) {
	return obj.length;
}
```
To learn the type of a variable, use `typeof`:

<table>
<thead>
<tr>
<th>Type</th>
<th>Predicate</th>
</tr>
</thead>
<tbody>
<tr>
<td>string</td>
<td><code>typeof s === "string"</code></td>
</tr>
<tr>
<td>number</td>
<td><code>typeof n === "number"</code></td>
</tr>
<tr>
<td>boolean</td>
<td><code>typeof b === "boolean"</code></td>
</tr>
<tr>
<td>undefined</td>
<td><code>typeof undefined === "undefined"</code></td>
</tr>
<tr>
<td>function</td>
<td><code>typeof f === "function"</code></td>
</tr>
<tr>
<td>array</td>
<td><code>Array.isArray(a)</code></td>
</tr>
</tbody>
</table>

This can be used with unions to make functions that accept unionized types as args and return different values depending on which type was passed:
```typescript
function wrapInArray(obj: string | string[]):
	if (typeof obj === "string") {
		return [obj]; 
	} else {
		return obj;
	}
```
The above returns the input unchanged if it is already an array containing a string,  wraps the input in an array if it is a string, and does not accept the input if it is not either of these types.

## Generics
Generics are a TS feature that allows you to give variables to a type. An object of type array with no generics could be an array containing anything, strings, numbers, functions objects etc, or a mixture of all of them. Generics allow you to describe exactly what the array should contain:
```typescript
type StringArray = Array<string>;
type NumberArray = Array<number>;
type UserArray = Array<{ name: string, id: number }>
```
 You can declare your own types using generics and do some interesting, flexible type allowances like this:
```typescript
interface Backpack<Type> {
	add: (obj: Type) => void;
	get: () => Type;
}

declare const backpack: Backpack<string>;

const object = backpack.get(); //string

backpack.add(23); //ERROR
```
The above defines an interface describing the shape of a backpack object, that has two properties which are both functions. It uses a generic to tell the TS compiler that backpack objects can have any type, that the `backpack.add()` functions should only accept arguments of that **SAME** type and that the `backpack.get()` method should return that same type. 

The second block uses the declare keyword to tell TS that a const named backpack exists, and has the Backpack type with it's variable type being string, meaning that the add method will only accept strings and the get method will return the string type object. (See here for clarification on the declare keyword in TS: [[Typescript - The Declare Keyword]]).


