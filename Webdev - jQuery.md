#frontend #frontend-libs #webdevSpecific 

**TLDR: jQuery lets you quickly and easily use prewritten JavaScript functions that achieve common or difficult tasks.** See the jQuery docs [here](https://learn.jquery.com/).

jQuery is the most popular JS tool of all time and was a foundational technology of the modern internet. These days it is on something of a slight decline in popularity, with tools like react being more in vouge to achieve what jQuery does, but it is nevertheless important to know: In 2022 jQuery was still used in 80 million websites, compared to Reacts 11 million.

## What is jQuery?
jQuery is a JavaScript framework designed to simplify HTML DOM tree traversal and manipulation, as well as event handling, CSS animation, and Ajax. It takes a lot of common tasks that require many lines of JavaScript to accomplish, and wraps them into methods that you can call with a single line of code.

jQuery also simplifies a lot of the complicated things from JavaScript, like AJAX calls and DOM manipulation.

## Including jQuery
Like bootstrap and Awesome Font, the easiest way to use jQuery in your projects is to include it with a CDN link. The most common CDN to use that hosts jQuery is google. Include it with the line below:
```html
<head>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.4/jquery.min.js"></script>
</head>
```

## Basic use of jQuery
This section will detail examples of basic use of jQuery selectors and functions to manipulate html elements and their css styling. These examples are adapted from the freeCodeCamp course on jQuery and refer loosely to the example that that course has you work through.

When using jQuery in an html document, a good place to start is with a document ready function. This is simply a jQuery statement that will execute code in the function only once the page DOM has been constructed and the html rendered, which prevents bugs from occuring when JS tries to manipulate page elements before they load. Here is an example of a document ready function in an html docs header:
```html
<html>
<head>
    <script src="https://code.jquery.com/jquery-1.9.1.min.js"></script>
    <script>
    $( document ).ready(function() {
        console.log( "document loaded" );
    });
    </script>
</head>
```
Similarly, code included inside `$( window ).on( "load", function() { ... })` will run once the entire page (images or iframes), not just the DOM, is ready.

All jQuery functions start with a `$`, usually referred to as a dollar sign operator, or as bling.
jQuery often selects an HTML element with a selector, then does something to that element, for example:
```js
$("button").addClass("animated bounce");
```
This code will select all button elements in the html document and give them the class "animated-bounce" which is a special css class from the Animate.css library. Note the syntax for the selector used to select all the button elements, and also note that the `.addClass()` function is a special function provided by jQuery. The `.removeClass()` function is also included and does what it sounds like.

In addition to targeting elements like in the above example, you can use the same syntax as css targeting to target elements of a specific class or id:
```js
$(".well").addClass("animated shake"); //Targets elements with class="well"
$("#target6").addClass("animated fadeOut"); // Targets elements with id="target6"
```

You can also use jQuery functions to edit the CSS and non-CSS properties of a target element or class of elements:
```html
<script>
	$(document).ready(function() {
		$("#target1").css("color", "red");
		$("#target1").prop("disabled", true);
	});
</script>
```

Using jQuery, you can change the text between the start and end tags of an element, or add additional HTML markup. The jQuery `.html()` function lets you add HTML tags and text within an element. Any content previously within the element will be completely replaced with the content you provide using this function.
```js
$("h3").html("<em>jQuery Playground</em>");
```

jQuery also has a similar function called `.text()` that only alters text without adding tags. In other words, this function will not evaluate any HTML tags passed to it, but will instead treat it as the text you want to replace the existing content with.

To remove a targeted element or class of elements, use the `.remove()` function:
```html
<script>
	$(document).ready(function() {
		$("#target4").remove();
	});
</script>
```

The `appendTo()` jQuery function allows you to select HTML elements and append them to another element. For example, if we wanted to move a button with `id="target4"` from a div with `id=right-well` to a div with `id=left-well` we would use:
```js
$("#target4").appendTo("#left-well");
```

In addition to moving elements, you can also copy them from one place to another using the `.clone()` function. For example, if we wanted to copy `target2` from the `left-well` div to the `right-well` div, we would use:
```js
$("#target2").clone().appendTo("#right-well");
```
Did you notice this involves sticking two jQuery functions together? This is called function chaining and it's a convenient way to get things done with jQuery.

Every HTML element has a `parent` element from which it `inherits` properties. The `parent()` function allows you to access the parent of whichever element you've selected with your jQuery selector. Here's an example of how you would use the `parent()` function if you wanted to give the parent element of the `left-well` element a background color of blue:
```js
$("#left-well").parent().css("background-color", "blue")
```

Similarly the `children()` jQuery function lets you access the child elements of the selected element. In this case it applies to all direct children elements, or elements that are one level below the selected element. The below example shows use of this function to change the css of all children of a div element with `id="left-well"`:
```js
$("#left-well").children().css("color", "blue")
```

To target a specific child of a given element in a situation where you can't necessarily target it directly by an id, you can exploit the fact that jQuery targets elements using CSS selectors, and use the `target:nth-child(n)` CSS selector to select all the nth elements with the target class or element type. Here's how you would give the third element in each well the bounce class:
```js
$(".target:nth-child(3)").addClass("animated bounce");
```

You can also target elements based on their positions using `:odd` or `:even` selectors. Note that jQuery is zero-indexed which means the first element in a selection has a position of 0. This can be a little confusing as, counter-intuitively, `:odd` selects the second element (position 1), fourth element (position 3), and so on.

Here's how you would target all the odd elements with class `target` and give them classes:
```js
$(".target:odd").addClass("animated shake");
```

Finally, you can target the whole page by targeting the body element. Here's how we would make the entire body fade out: `$("body").addClass("animated fadeOut");`