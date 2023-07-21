#typescript 

One of TS's central conceits is that type checking should focus on the shape of variables, meaning the structure or organization of data. This is known as structural typing, or sometimes Duck typing.

In a structural type system like TS uses, if two objects have the same shape, then they are considered to have the same type.
```typescript
interface Point {
	x: number;
	y: number;
}

function logPoint(p: Point) {
	console.log(`{p.x}, {p.y}`);
}

const myPoint = {x: 12, y: 36};
logPoint(myPoint);
```
The last line in the above will log "12, 36". Even though the variable `myPoint` was never declared to have the type `Point`, the argument for the `logPoint()` function was. So TS compares the shape of the `myPoint` variable to the `Point` type and concludes that it fits because it has the same shape.

Note that the system only requires a sub-set of the objects field to match, so a variable with no x or y would not work but one with x, y and z would.

