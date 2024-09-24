#webdevSpecific 

The example notes app discussed in previous FSO content, like the example apps built for the exercises, have so far not been styled. This means that all the content has been laid out and styled with the browser standard defaults for unstyled HTML.

Through FSO content we will look at several ways we can add styles to React apps, starting here with the simplest. The first method covered will be adding CSS the old school way, with a single file and no CSS preprocessor. To get started on this, add a new file called `index.css` to the root of the `src` directory.

NOTE: this section is a fairly quick run-down of very simple CSS basics, skip it if you are reviewing these notes and have not just come back from a million year break.

## Styling basics
CSS at it's core is a system of **selectors** and **declarations**. Declarations are the styling rules themselves, and set the properties of an element. They are applied to elements or groups of elements on the HTML page document that match the selector in which they are declared. In the below rule, the selector hits all `<h1>` elements, and changes their color to green:
```css
h1 {
	color: green;
}
```

One CSS rule can contain an arbitrary number of declarations for properties. You can set the color, font-style and font-weight for all `<h1>` elements in one go with a rule like this:
```css
h1 {
	color: green;
	font-style: italic;
	font-weight: bold;
}
```

There are a bunch of different ways to select elements for the application of CSS rules. In the notes application example, because all note elements are enclosed in `<li>` tags under the hood, you could style every note with as rule that hits `<li>` tags:
```css
li {
  color: grey;
  padding-top: 3px;
  font-size: 15px;
}
```

Targeting element types is problematic though, as it can be too non-specific in larger apps. Class selectors help here, where you give an HTML element a `class` attribute, then hit it with a rule like that starts with a period. In a notes app you might give all notes the same class like this:
```jsx
const Note = ({ note, toggleImportance }) => {
  const label = note.important 
    ? 'make not important' 
    : 'make important';

  return (

    <li className='note'>
      {note.content} 
      <button onClick={toggleImportance}>{label}</button>
    </li>
  )
}
```

Then you can hit them with a rule like this:
```css
.note {
  color: grey;
  padding-top: 5px;
  font-size: 15px;
}
```
This method will avoid styling `<li>` elements that are not notes.

## Improved error messages
