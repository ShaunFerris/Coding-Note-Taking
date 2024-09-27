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
In previous work on the example notes app for FSO content, we implemented an error msg that was displayed when the user tried to toggle the importance of an already deleted note.

This functionality could be pulled out into it's own component:
```jsx
const Notification = ({ message }) => {
	if (message === null) {
		return null	
	}

	return (
		<div className='error'>
			{message}	
		</div>	
	)
}
```
If the value of the `message` prop is `null`, then nothing is rendered to the screen by this component, as `null` is returned. If it is not `null`, then the message is rendered in a `<div>`.

We can then add an `errorMessage` state to the main `App` component, and pass this as props to the Notification component:
```jsx
const App = () => {
  const [notes, setNotes] = useState([]) 
  const [newNote, setNewNote] = useState('')
  const [showAll, setShowAll] = useState(true)

  const [errorMessage, setErrorMessage] = useState('some error happened...')

  // ...

  return (
    <div>
      <h1>Notes</h1>
      <Notification message={errorMessage} />
      <div>
        <button onClick={() => setShowAll(!showAll)}>
          show {showAll ? 'important' : 'all' }
        </button>
      </div>      
      // ...
    </div>
  )
}
```
The notification can then be given a style that is appropriate for an error msg:
```css
.error {
  color: red;
  background: lightgrey;
  font-size: 20px;
  border-style: solid;
  border-radius: 5px;
  padding: 10px;
  margin-bottom: 10px;
}
```
Finally, edit the `toggleImportanceOf` function to set an error message if the call to `noteService.update()` fails:
```jsx
  const toggleImportanceOf = id => {
    const note = notes.find(n => n.id === id)
    const changedNote = { ...note, important: !note.important }

    noteService
      .update(id, changedNote).then(returnedNote => {
        setNotes(notes.map(note => note.id !== id ? note : returnedNote))
      })
      .catch(error => {
        setErrorMessage(
          `Note '${note.content}' was already removed from server`
        )
        setTimeout(() => {
          setErrorMessage(null)
        }, 5000)
        setNotes(notes.filter(n => n.id !== id))
      })
  }
```

## Inline styles
React also allows you to write styles directly in the code (inline) in the same way that you can with HTML elements in a vanilla site.

Css rules are defined differently in JS than in normal CSS files, see below for an example:
```css
// styles in a CSS files
{
  color: green;
  font-style: italic;
  font-size: 16px;
}
```
```CSS
// the same styles as above applied inline in JS
{
  color: 'green',
  fontStyle: 'italic',
  fontSize: 16
}
```

Every CSS property is defined as a separate property of the JavaScript object. Numeric values for pixels can be simply defined as integers. One of the major differences compared to regular CSS, is that hyphenated (kebab case) CSS properties are written in camelCase.

We could make a Footer component for the example notes app and add inline styles to it using a JS object, like this:
```jsx
const Footer = () => {
  const footerStyle = {
    color: 'green',
    fontStyle: 'italic',
    fontSize: 16
  }
  return (
    <div style={footerStyle}>
      <br />
      <em>Note app, Department of Computer Science, University of Helsinki 2024</em>
    </div>
  )
}

const App = () => {
  // ...

  return (
    <div>
      <h1>Notes</h1>

      <Notification message={errorMessage} />

      // ...  


      <Footer />
    </div>
  )
}
```

Inlines styles come with some limitations. For instance, so-called pseudo-classes can't be used in a straightforward way.

(Below taken verbatim from FSO notes:)
Inline styles and some of the other ways of adding styles to React components go completely against the grain of old conventions. Traditionally, it has been considered best practice to entirely separate CSS from the content (HTML) and functionality (JavaScript). According to this older school of thought, the goal was to write CSS, HTML, and JavaScript into their separate files.

The philosophy of React is, in fact, the polar opposite of this. Since the separation of CSS, HTML, and JavaScript into separate files did not seem to scale well in larger applications, React bases the division of the application along the lines of its logical functional entities.

The structural units that make up the application's functional entities are React components. A React component defines the HTML for structuring the content, the JavaScript functions for determining functionality, and also the component's styling; all in one place. This is to create individual components that are as independent and reusable as possible.

