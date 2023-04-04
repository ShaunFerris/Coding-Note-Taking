#javascript #regex 

## Testing with REGEX
JavaScript has multiple ways to use regexes. One way to test a regex is using the `.test()` method. The `.test()` method takes the regex, applies it to a string (which is placed inside the parentheses), and returns `true` or `false` if your pattern finds something or not.
```js
let testStr = "freeCodeCamp";
let testRegex = /Code/;
testRegex.test(testStr);
```

In the above case the test method will return true.

This is powerful to search single strings, but it's limited to only one pattern. You can search for multiple patterns using the `alternation` or `OR` operator: `|`. This operator matches patterns either before or after it. For example, if you wanted to match the strings `yes` or `no`, the regex you want is `/yes|no/`. You can also search for more than just two patterns. You can do this by adding more patterns with more `OR` operators separating them, like `/yes|no|maybe/`.

You can match both cases using what is called a flag. There are other flags but here you'll focus on the flag that ignores case - the `i` flag. You can use it by appending it to the regex. An example of using this flag is `/ignorecase/i`. This regex can match the strings `ignorecase`, `igNoreCase`, and `IgnoreCase`.

## Matching with REGEX
So far, you have only been checking if a pattern exists or not within a string. You can also extract the actual matches you found with the `.match()` method. To use the `.match()` method, apply the method on a string and pass in the regex inside the parentheses.

Here's an example:
```js
"Hello, World!".match(/Hello/);
let ourStr = "Regular expressions";
let ourRegex = /expressions/;
ourStr.match(ourRegex);
```
Here the first `match` would return `["Hello"]` and the second would return `["expressions"]`.

Note that the `.match` syntax is the "opposite" of the `.test` method you have been using thus far:
```js
'string'.match(/regex/);
/regex/.test('string');
```

To search or extract a pattern more than once, you can use the global search flag: `g`.
```js
let repeatRegex = /Repeat/g;
testStr.match(repeatRegex);
```
And here `match` returns the value `["Repeat", "Repeat", "Repeat"]`
The flag `i` will make your regex search case agnostic.

## WIldcard matching
Sometimes you won't (or don't need to) know the exact characters in your patterns. Thinking of all words that match, say, a misspelling would take a long time. Luckily, you can save time using the wildcard character: `.`

The wildcard character `.` will match any one character. The wildcard is also called `dot` and `period`. You can use the wildcard character just like any other character in the regex. For example, if you wanted to match `hug`, `huh`, `hut`, and `hum`, you can use the regex `/hu./` to match all four words.
```js
let humStr = "I'll hum a song";
let hugStr = "Bear hug";
let huRegex = /hu./;
huRegex.test(humStr);
huRegex.test(hugStr);
```
Both of these `test` calls would return `true`.

## Character classes
The wild card character and literals used so far are the extremes of REGEX, with one matching anything and the other matchin exactly one thing. You can add some middle ground flexibility using character classes.

Character classes allow you to define a group of characters you wish to match by placing them inside square (`[` and `]`) brackets.

For example, you want to match `bag`, `big`, and `bug` but not `bog`. You can create the regex `/b[aiu]g/` to do this. The `[aiu]` is the character class that will only match the characters `a`, `i`, or `u`.
```js
let bigStr = "big";
let bagStr = "bag";
let bugStr = "bug";
let bogStr = "bog";
let bgRegex = /b[aiu]g/;
bigStr.match(bgRegex);
bagStr.match(bgRegex);
bugStr.match(bgRegex);
bogStr.match(bgRegex);
```
In order, the four `match` calls would return the values `["big"]`, `["bag"]`, `["bug"]`, and `null`.
You can also use ranges like `[a-z]` which will include the entire lowercase alphabet, or `[1-9]`.
You can also search multiple ranges like this: `[0-9a-g]`.

## Negated character classes
To create a negated character set, you place a caret character (`^`) after the opening bracket and before the characters you do not want to match.

For example, `/[^aeiou]/gi` matches all characters that are not a vowel. Note that characters like `.`, `!`, `[`, `@`, `/` and white space are matched - the negated vowel character set only excludes the vowel characters.

## Repetition characters
You can use `+` and `*` to match one or more, or zero or more instances of the character to their left respectively. For example the regex `/a+/g` would find one match in the string `aabc` where `/a/g` would find two.

## Greedy and lazy matching
In regular expressions, a greedy match finds the longest possible part of a string that fits the regex pattern and returns it as a match. The alternative is called a lazy match, which finds the smallest possible part of the string that satisfies the regex pattern. **Regular expressions are greedy by default.**

You can apply the regex `/t[a-z]*i/` to the string `"titanic"`. This regex is basically a pattern that starts with `t`, ends with `i`, and has some letters in between. Regular expressions are by default greedy, so the match would return `["titani"]`. It finds the largest sub-string possible to fit the pattern. However, you can use the `?` character to change it to lazy matching. `"titanic"` matched against the adjusted regex of `/t[a-z]*?i/` returns `["ti"]`.

## Start and end of string matching
When used in a character class set the caret, `^`, negates, but when used outside a character class set the caret character is used to denote that search should begin from the start of the string.
```js
let firstString = "Ricky is first and can be found.";
let firstRegex = /^Ricky/;
firstRegex.test(firstString);
let notFirst = "You can't find Ricky now.";
firstRegex.test(notFirst);
```
The first `test` call would return `true`, while the second would return `false`.

You can search the end of strings using the dollar sign character `$` at the end of the regex.
```js
let theEnding = "This is a never ending story";
let storyRegex = /story$/;
storyRegex.test(theEnding);
let noEnding = "Sometimes a story will have to end";
storyRegex.test(noEnding);
```
The first `test` call would return `true`, while the second would return `false`.


