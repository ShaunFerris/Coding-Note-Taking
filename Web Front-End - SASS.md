#webdevSpecific #HTML #CSS #frontend #frontend-libs 

SASS stands for syntactically awesome style sheets, and is an extension of CSS. More specifically it is a preprocessor scripting language that is interpereted or compiled into CSS. SASS adds features that aren't available in basic CSS, which make it easier for you to simplify and maintain the style sheets for your projects.

## Install and use of SASS
Because sass is a preprocessor language, you need to install a compiler for it, and it will compile your .scss files into .css files that will actually be linked in your HTML. The two main ways to compile sass are to use the command line tool installed through npm with the command:
```bash
npm install -g sass
```

Or to use the vs code extension "Live SASS compiler" which will allow you to set up a file location is your project directory for the compiled css to go, and will live compile any additions you make to the .scss file.

It is also worth noting that SASS has two different valid syntaxes with different associated file formats. The original syntax is used in .sass files, and uses indentation instead of the traditional semi-colons and curly braces of css. The SCSS syntax, used in .scss (Sassy css) files, is a superset of normal css and uses the traditional syntax. Either way, both file types compile down to css so it is ultimately a user choice.

## Store data with SASS variables
One major difference of sass compared to css is that it allows you to declare and store data in variables for later use and reuse, similar to how you would do so in a programming language. Where you would declare a variable with the `let` or `const` keywords in JS, variables in sass are declared with a bling, eg: 
```scss
$main-fonts: Arial, sans-serif;
$headings-color: green;
```
And stored variables are also called with the bling as part of their name:
```scss
h1 {
  font-family: $main-fonts;
  color: $headings-color;
}
```
This is obviously a useful feature, especially for uses such as easily changing the color of a large number of elements that you want to be colored the same.

Note that this can apparently be done in basic CSS aswell, [see the docs](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties).

## Nesting CSS with SASS
Sass allows css rules to be nested within one another, whereas css needs every element selector to be on a different line. This becomes very useful for organization and readabilitly when working with large files with lots and lots of css selectors and rules for different elements. It is especially useful for example in that it allows you to keep the rules for child elements clearly and visually associated with their parent elements selector and rules:
```scss
nav {
  background-color: red;

  ul {
    list-style: none;

    li {
      display: inline-block;
    }
  }
}
```

## Create mixins
In sass, a mixin is a collection of css statements that can be reused throughout the style sheet. Mixins are defined in a similar way to functions.

A common use-case for sass mixins is in the situation where we are using a relatively new css feature. Newer CSS features take time before they are fully adopted and ready to use in all browsers. As features are added to browsers, CSS rules using them may need vendor prefixes. Consider `box-shadow` (box-shadow doesn't need vendor prefixes anymore but did when it was first introduced):
```scss
div {
  -webkit-box-shadow: 0px 0px 4px #fff;
  -moz-box-shadow: 0px 0px 4px #fff;
  -ms-box-shadow: 0px 0px 4px #fff;
  box-shadow: 0px 0px 4px #fff;
}
```
It's a lot of typing to re-write this rule for all the elements that have a `box-shadow`, or to change each value to test different effects. Mixins are like functions for CSS. Here is how to write one:
```scss
@mixin box-shadow($x, $y, $blur, $c){ 
  -webkit-box-shadow: $x $y $blur $c;
  -moz-box-shadow: $x $y $blur $c;
  -ms-box-shadow: $x $y $blur $c;
  box-shadow: $x $y $blur $c;
}
```
The definition starts with `@mixin` followed by a custom name. The parameters (the `$x`, `$y`, `$blur`, and `$c` in the example above) are optional. Now any time a `box-shadow` rule is needed, only a single line calling the mixin replaces having to type all the vendor prefixes. A mixin is called with the `@include` directive:
```scss
div {
  @include box-shadow(0px, 0px, 4px, #fff);
}
```

## Add logic to your stylesheets
The `@if` directive in Sass is useful to test for a specific case - it works just like the `if` statement in JavaScript.
```scss
@mixin make-bold($bool) {
  @if $bool == true {
    font-weight: bold;
  }
}
```

And just like in JavaScript, the `@else if` and `@else` directives test for more conditions:
```scss
@mixin text-effect($val) {
  @if $val == danger {
    color: red;
  }
  @else if $val == alert {
    color: yellow;
  }
  @else if $val == success {
    color: green;
  }
  @else {
    color: black;
  }
}
```
These add real function like power to sass mixins.

Similarly, this logic can be extended to adding for loops to your sass mixins using the `@for` directive. `@for` is used in two ways: "start through end" or "start to end". The main difference is that the "start **to** end" _excludes_ the end number as part of the count, and "start **through** end" _includes_ the end number as part of the count.

Here's a start **through** end example:
```scss
@for $i from 1 through 12 {
  .col-#{$i} { width: 100%/12 * $i; }
}
```
The `#{$i}` part is the syntax to combine a variable (`i`) with text to make a string. When the Sass file is converted to CSS, it looks like this:
```scss
.col-1 {
  width: 8.33333%;
}

.col-2 {
  width: 16.66667%;
}

...

.col-12 {
  width: 100%;
}
```
This is a powerful way to create a grid layout. Now you have twelve options for column widths available as CSS classes.

Sass also offers the `@each` directive which loops over each item in a list or map. On each iteration, the variable gets assigned to the current value from the list or map.
```scss
@each $color in blue, red, green {
  .#{$color}-text {color: $color;}
}
```

A map has slightly different syntax. Here's an example:
```scss
$colors: (color1: blue, color2: red, color3: green);

@each $key, $color in $colors {
  .#{$color}-text {color: $color;}
}
```
Note that the `$key` variable is needed to reference the keys in the map. Otherwise, the compiled CSS would have `color1`, `color2`... in it. Both of the above code examples are converted into the following CSS:
```scss
.blue-text {
  color: blue;
}

.red-text {
  color: red;
}

.green-text {
  color: green;
}
```

The `@while` directive is an option with similar functionality to the JavaScript `while` loop. It creates CSS rules until a condition is met.

The `@for` example given earlier created a simple grid system. This can also work with `@while`.
```scss
$x: 1;
@while $x < 13 {
  .col-#{$x} { width: 100%/12 * $x;}
  $x: $x + 1;
}
```
First, define a variable `$x` and set it to 1. Next, use the `@while` directive to create the grid system _while_ `$x` is less than 13. After setting the CSS rule for `width`, `$x` is incremented by 1 to avoid an infinite loop.

