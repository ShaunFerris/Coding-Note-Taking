#typescript 

NOTE: this note is a useful stackoverflow explaination that I found [here](https://stackoverflow.com/questions/43335962/purpose-of-declare-keyword-in-typescript). Credit to [user1852503](https://stackoverflow.com/users/1852503/user1852503)
## TL;DR
`declare` is used to tell the compiler "this thing (usually a variable) exists already, and therefore can be referenced by other code, also there is no need to compile this statement into any JavaScript".

## Common Use Case:
You add a reference to your web page to a JavaScript file that the compiler knows nothing about. maybe it is a script coming from some other domain like 'foo.com'. when evaluated the script will create an object with some useful API methods and assign it to the identifier 'fooSdk' on the global scope.

You want your TypeScript code to be able to call `fooSdk.doSomething()`, but since your compiler does not know `fooSdk` variable exists, you will get a compilation error.

You then use the `declare` keyword as a way of telling the compiler "trust me, this variable exists and has this type". The compiler will use this statement to statically check other code but will not trans-compile it into any JavaScript in the output.
```javascript
declare const fooSdk = { doSomething: () => boolean }
```

Newer Typescript version require a slightly different syntax:
```javascript
declare const fooSdk : { doSomething: () => boolean }
```

In the same vein, you can add the declare keyword to class properties to tell the compiler not to emit any code that would create this property, assuming you have your own code that would create it that the compiler does not know about or does not understand.

Your specific example is different since you are declaring a type, not a variable, types already do not compile into any JavaScript. I do not know if there is any reason to declare a type.