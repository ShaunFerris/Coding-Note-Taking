#javascript #testing 

Cypress provides the ability to conduct component testing. Similar to unit tests, component tests allow you to test the behaviour of discrete units of code, but are slightly more complex in scope because they go beyond single functions or modules. Cypress is capable of conducting these tests on components built with any of the major JS front end libraries like React, Vue etc.

## Simple example
Here is a minimal example that gets the idea across, mounting a button component and checking that it displays the correct text:
```typescript
import Button from "./Button"

it('uses custom text for the button label', () => {
  cy.mount(<Button>Click me!</Button>)
  cy.get('button').should('contains.text', 'Click me!')
})
```
As you can see the test mounts the component and gives it text to display, then assets that it displays correctly. This is the general idea of component tests, checking on the behaviour of your components while they are decoupled from the wider system you built them for.