#webdevSpecific #HTML #CSS #frontend

## What are HTML and CSS?
HTML stands for HperText Markup Language and is the standard markup language for documents designed to be displayed in a web browser. Web browsers recieve an html document either from a server or from local storage, and render the webpage based on the html instructions. The html describes semantically the structure and contents of the web page, by describing elements of the webpage such as paragraphs and headings, delineated with tags.

CSS stands for Cascading Style Sheets, and is an accessory language to HTML, describing the presentation of the elements listed in the html document.

Essentially, html is the main surface along which we interact with the browser, and is used to construct the DOM (see [[Webdev - Hub]]). The html includes a reference to the CSS document, which details the styling for classes of tag in the HTML doc.

## Basic HTML syntax and tags
HTML uses tags to define the structure of a web page. A tag consists of an opening and a closing tag, with content in between. The basic structure of an HTML document includes the `<!DOCTYPE>` declaration, the `<html>` element, the `<head>` element, and the `<body>` element.

### Intermediate Level Use with Examples
Intermediate level use of HTML can involve creating more complex layouts and elements, such as forms, tables, and lists. For example, to create a form in HTML, you can use the `<form>` element and its associated attributes, such as `action` and `method`, along with input fields such as `<input>` and `<textarea>`. Here's an example:
```html
<form action="/submit-form" method="post">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name">
  <label for="email">Email:</label>
  <input type="email" id="email" name="email">
  <label for="message">Message:</label>
  <textarea id="message" name="message"></textarea>
  <button type="submit">Send</button>
</form>
```
The basic things to remember about HTML syntax are to delineate the document with elements, elements are created with tags, and each tag can have an id and a class to identify it in the css or js. Ids must be unique among elements, but classes can be shared among many elements.

### Further Reading
If you're interested in learning more about HTML, check out the following resources:

-   [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTML)
-   [W3Schools HTML Tutorial](https://www.w3schools.com/html/)
-   [HTML5 Doctor](http://html5doctor.com/)

## CSS Summary
CSS (Cascading Style Sheets) is a stylesheet language used to describe the presentation of HTML or XML documents. It allows developers to define styles for web pages, including layout, colors, fonts, and other design aspects.

### Syntax and Structure
CSS uses selectors to target HTML elements and apply styles to them. A CSS rule consists of a selector and a declaration block, which contains one or more declarations that specify the style properties and their values. The basic structure of a CSS file includes the selector and the declaration block in curly braces `{}`.

### Intermediate Level Use with Examples
Intermediate level use of CSS can involve creating more complex layouts and styles, such as responsive designs, animations, and custom fonts. For example, to create a responsive layout in CSS, you can use media queries and CSS Grid or Flexbox. Here's an example:
```css
@media screen and (min-width: 768px) {
  .container {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
  }
}

@media screen and (max-width: 767px) {
  .container {
    display: flex;
    flex-direction: column;
  }
}
```

### Further Reading
If you're interested in learning more about CSS, check out the following resources:

-   [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/CSS)
-   [W3Schools CSS Tutorial](https://www.w3schools.com/css/)
-   [CSS-Tricks](https://css-tricks.com/)
