#javascript #testing 

In software generally, the term stub means a piece of code that stands in for or emulates some other piece of code. In testing specifically, stubs provide a canned answer to some call or query made during a testing flow, and usually don't respond to anything outside the scope of the test.

Stubbing is most commonly used for unit tests but can be applied in useful ways to e2e and integration tests too.

## The stub function in Cypress
Cypress provides a built-in `cy.stub()` function that allows you to directly stub a function, telling cypress what output to emulate for a given function call. Below are some examples taken from the Cypress docs:
```typescript
// create a standalone stub (generally for use in unit test)
cy.stub()

// replace obj.method() with a stubbed function
cy.stub(obj, 'method')

// force obj.method() to return "foo"
cy.stub(obj, 'method').returns('foo')

// force obj.method() when called with "bar" argument to return "foo"
cy.stub(obj, 'method').withArgs('bar').returns('foo')

// force obj.method() to return a promise which resolves to "foo"
cy.stub(obj, 'method').resolves('foo')

// force obj.method() to return a promise rejected with an error
cy.stub(obj, 'method').rejects(new Error('foo'))
```

## Spying
Similar to how stubbing allows the emulation of function calls, spying allows you to watch a given function,  and assert that it was called with the correct args, or returned the correct value. A spy does **not** modify the behavior of the function - it is left perfectly intact. A spy is most useful when you are testing the contract between multiple functions and you don't care about the side effects the real function may create (if any).

```typescript
cy.spy(obj, 'method')
```

## Intercept for stubbing and spying
`cy.intercept()` is a function provided by Cypress that allows you to grab a network request or reponse and either stub it or spy on it. See the [docs](https://docs.cypress.io/api/commands/intercept)for more information on the ins and outs of it.

Intercept was applied to homeHub to allow me to stub the token that the user gets on login through nextAuth, allowing me to test routes that require the user to be logged in. This was done by intercepting requests to the `host/api/auth` route using a custom login mock function:
```typescript
Cypress.Commands.add("login", (username, password) => {
  cy.session([username, password], () => {
    cy.intercept("/api/auth/session", { fixture: "session.json" }).as(
      "session"
    );
    /**
     * Set the cookie for Cypress.
     * Must be a valid cookie so that Next-Auth can decode.
     * Cookie must be refreshed when it expires. Keep an eye out for
     * future fixes to this workaround.
     */
    cy.setCookie(
      "next-auth.session-token",
      Cypress.env("TEST_SESSION_COOKIE")
    );
  });
});
```
If you look at the `cy.intercept()` call on line three, you see that it has the api endpoint as it's first arg and references a fixture which contains a mock user session token with which to stub any requests asking for the user session token.