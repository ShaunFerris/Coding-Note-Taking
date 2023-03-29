#javascript 

Switch statements are an alternative structure to complex if/else logic, useful for selecting from many possible cases which you want to respond to by running different code. In python 3.10 and greater, the equivalent is [[Python - Structural Pattern Matching]], also called match case statements.

The general structure of switch statements is below, they follow the same basic rules for formatting as if else statements and most other JS code blocks, but use colons for each case [[JavaScript Basics]]:
```
switch (lowerCaseLetter) {
	case "a":
		console.log("A");
		break;
	case "b":
		console.log("B");
		break;
	default:
		console.log("It wasn't and A or a B")
}
```
Switch statements should always end in a default case  as in the above example.

Cases in switch statements are tested with strict equality, and the breaks are necessary to prevent triggering the default case. Because of the breaks used in switch statements it is worth thinking about instances where and input may match multiple cases and being aware that only the first match will run a response.