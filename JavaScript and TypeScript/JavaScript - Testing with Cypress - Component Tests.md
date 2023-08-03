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

The cypress browser based testing system is also great for this kind of testing as it allows you to see what the components looks like visually while testing it.

## Component tests vs end to end tests
In short, Cypress Component Testing uses the same test runner, commands, and API to test components instead of pages.

The primary difference is that Cypress Component Testing builds your components using a development server instead of rendering within a complete website, which results in faster tests and fewer dependencies on infrastructure than end-to-end tests covering the same code paths.

Keep in mind that both component testing and e2e testing allow for integration tests, but in slightly different ways. Components testing can test integration of components, where e2e tests can test the wider interactions of different modules.

## Getting started with component tests
<span style="color: cyan; font-weight: bold;">You can pass props to components in test:</span>
```typescript
it('supports a "count" prop to set the value', () => {
  cy.mount(<Stepper count={100} />)
  cy.get('[data-cy=counter]').should('have.text', '100')
})
```

<span style="color: cyan; font-weight: bold;">You can test user interaction with a component:</span>
```typescript
it('when the increment button is pressed, the counter is incremented', () => {
  cy.mount(<Stepper />)
  cy.get('[data-cy=increment]').click()
  cy.get('[data-cy=counter]').should('have.text', '1')
})

it('when the decrement button is pressed, the counter is decremented', () => {
  cy.mount(<Stepper />)
  cy.get('[data-cy=decrement]').click()
  cy.get('[data-cy=counter]').should('have.text', '-1')
})
```

<span style="color: cyan; font-weight: bold;">You can test components with events:</span>
For example if you want to check that the stepper component triggers an event on click. You can do this using spies:
```typescript
it('clicking + fires a change event with the incremented value', () => {
  const onChangeSpy = cy.spy().as('onChangeSpy')
  cy.mount(<Stepper onChange={onChangeSpy} />)
  cy.get('[data-cy=increment]').click()
  cy.get('@onChangeSpy').should('have.been.calledWith', 1)
})
```

The next step in learning component testing is to style the components for testing. See here: [[JavaScript - Testing with Cypress - Styling Components for Tests]]

