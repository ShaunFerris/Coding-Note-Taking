#webdevSpecific 

## Displaying the login form conditionally
Going back to the theroetical example notes app, lets modify it so that the form for user login is not shown all the time, but only when the user wants to login in. We will make it so there is a button to display the login form, and if it is displayed, another button to hide it. Before implementing this behaviour we should start by extracting the login form into it's own component:
```jsx
const LoginForm = ({
   handleSubmit,
   handleUsernameChange,
   handlePasswordChange,
   username,
   password
  }) => {
  return (
    <div>
      <h2>Login</h2>
      <form onSubmit={handleSubmit}>
        <div>
          username
          <input
            value={username}
            onChange={handleUsernameChange}
          />
        </div>
        <div>
          password
          <input
            type="password"
            value={password}
            onChange={handlePasswordChange}
          />
      </div>
        <button type="submit">login</button>
      </form>
    </div>
  )
}

export default LoginForm
```

In the above implementation, all the state and functions that the component needs are passed as props from the parent component, in this case the `App` component in the `App.jsx` file. Note that the props are destructured into variables straight out of the props object.

One way to quickly implement the desired functionality for the login form is to edit the `App` component like this:
```jsx
const App = () => {
  const [loginVisible, setLoginVisible] = useState(false)

  // ...

  const loginForm = () => {
    const hideWhenVisible = { display: loginVisible ? 'none' : '' }
    const showWhenVisible = { display: loginVisible ? '' : 'none' }

    return (
      <div>
        <div style={hideWhenVisible}>
          <button onClick={() => setLoginVisible(true)}>log in</button>
        </div>
        <div style={showWhenVisible}>
          <LoginForm
            username={username}
            password={password}
            handleUsernameChange={({ target }) => setUsername(target.value)}
            handlePasswordChange={({ target }) => setPassword(target.value)}
            handleSubmit={handleLogin}
          />
          <button onClick={() => setLoginVisible(false)}>cancel</button>
        </div>
      </div>
    )
  }

  // ...
}
```
**Personally I hate this, becuase why would you need a loginForm function in the app component when you have extracted this, I think this is dumb as fuck**

The _App_ component state now contains the boolean _loginVisible_, which defines if the login form should be shown to the user or not.

The value of _loginVisible_ is toggled with two buttons. Both buttons have their event handlers defined directly in the component:
```js
<button onClick={() => setLoginVisible(true)}>log in</button>

<button onClick={() => setLoginVisible(false)}>cancel</button>
```

The visibility of the component is defined by giving the component an [inline](https://fullstackopen.com/en/part2/adding_styles_to_react_app#inline-styles) style rule, where the value of the [display](https://developer.mozilla.org/en-US/docs/Web/CSS/display) property is _none_ if we do not want the component to be displayed:
```js
const hideWhenVisible = { display: loginVisible ? 'none' : '' }
const showWhenVisible = { display: loginVisible ? '' : 'none' }

<div style={hideWhenVisible}>
  // button
</div>

<div style={showWhenVisible}>
  // button
</div>
```

We are once again using the "question mark" ternary operator. If _loginVisible_ is _true_, then the CSS rule of the component will be:
```css
display: 'none';
```
If _loginVisible_ is _false_, then _display_ will not receive any value related to the visibility of the component.

## Components and their children, aka props.children
