#frontend #frontend-libs #webdevSpecific 

**TLDR: jQuery lets you quickly and easily use prewritten JavaScript functions that achieve common or difficult tasks. **

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

