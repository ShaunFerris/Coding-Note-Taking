#javascript #testing 

Once Cypress has been properly installed and started in a project, the next step is usually to create your first E2E test. You can do this by running cypress, selecting end to end testing, launching a browser and then selecting the "Create new spec" button.

You will be prompted to name the file that will be created, but just accept the default name for now if it is your first test.

Once we've created that file, you should see it immediately displayed in the list of end-to-end specs in the Cypress browser window. Cypress monitors your spec files for any changes and automatically displays any changes. Even though we haven't written any code yet - that's okay - let's click on your new spec and watch Cypress launch it. Spoiler alert: it's probably going to **FAIL**. Don't worry, it's just because you haven't set up Cypress to visit a page in your app yet! Let's try something different.

## Writing your first E2E test
The default test provided by Cypress does actually pass, and it has the following code:
```typescript
describe("template spec", () => {
  it("passes", () => {
    cy.visit("https://example.cypress.io");
  });
});
```
We can break down what this does line by line:
1. `describe()` is a function provided by Cypress that is used to group test cases. It allows for nesting of tests within groups as deep as you like. The function takes two arguments, a string to describe or name the group of tests and a call back function that runs the actual test cases in the group
2. The `it()` function describes individula test cases inside the `describe()` functions callback argument. Similarly to `describe()` it takes two arguments, a name for the test and a callback function that outlines what the test should do
3. The `cy.visit()` function tells Cypress that we want to navigate to a url. In the default test this command is used to go to a test URL setup by Cypress, and on successful navigation the test will return true and pass.

When this test is run in cypress you will see this screen, showing the test spec file, the tests status (passing in this case) and the browser view, which here is the test webpage provided by Cypress:
![[Pasted image 20230728101653.png]]
This testing screen live updates from the project directory, so when I go into my editor and rename the test to "basic content testing", that is immediately reflected in the Cypress browser window. If I add an assertion to the test that expects true to evalute to false like so:
```typescript
describe("basic content testing", () => {
  it("passes", () => {
    cy.visit("https://example.cypress.io");
    expect(true).to.equal(false);
  });
});
```
You can see that the test is now failing:
![[Pasted image 20230728102253.png]]
Now lets re-write this basic content test to run some simple end to end tests on the Cypress example page. There is a link on the page that says "type" and we are going to write a test for clicking on this link. We start by finding the link with `cy.contains()`:
```typescript
describe('My First Test', () => {
  it('finds the content "type"', () => {
    cy.visit('https://example.cypress.io')

    cy.contains('type')
  })
})
```
This should pass, even without an assertion, because the command is built to fail if it does not find what it is looking for. If we change it to hype, which is not a word on the page, it will fail but only after a few seconds as Cypress is automatically waiting and retrying because it expects the content to eventually be found in the DOM. To actually click on the link that we have identified in the site we chain the click method like this:
```typescript
describe('My First Test', () => {
  it('clicks the link "type"', () => {
    cy.visit('https://example.cypress.io')

    cy.contains('type').click()
  })
})
```
The app preview in the Cypress browser should also have updated at this point to reflect that the link has actually been followed. This means that you can now have the test assert something about this page!

To assert that the new page navigated to should conform to a specific URL you would write this:
```typescript
describe('My First Test', () => {
  it('clicking "type" navigates to a new url', () => {
    cy.visit('https://example.cypress.io')

    cy.contains('type').click()

    // Should be on a new URL which
    // includes '/commands/actions'
    cy.url().should('include', '/commands/actions')
  })
})
```
We are not limited to a single interaction and assertion in a given test. In fact, many interactions in an application may require multiple steps and are likely to change your application state in more than one way.

We can continue the interactions and assertions in this test by adding another chain to interact with and verify the behavior of elements on this new page.

We can use [cy.get()](https://docs.cypress.io/api/commands/get) to select an element based on its class. Then we can use the [.type()](https://docs.cypress.io/api/commands/type) command to enter text into the selected input. Finally, we can verify that the value of the input reflects the text that was typed with another [.should()](https://docs.cypress.io/api/commands/should).

In general, the structure of your test should flow query -> query -> command or assertion(s). It's best practice not to chain anything after an action command; for more details on why this is, see our guide on [retry-ability](https://docs.cypress.io/guides/core-concepts/retry-ability).
```typescript
describe('My First Test', () => {
  it('Gets, types and asserts', () => {
    cy.visit('https://example.cypress.io')

    cy.contains('type').click()

    // Should be on a new URL which
    // includes '/commands/actions'
    cy.url().should('include', '/commands/actions')

    // Get an input, type into it
    cy.get('.action-email').type('fake@email.com')

    //  Verify that the value has been updated
    cy.get('.action-email').should('have.value', 'fake@email.com')
  })
})
```
And there you have it: a short test in Cypress that visits a page, finds and clicks a link, verifies the URL and then verifies the behavior of an element on the new page. If we read it out loud, it might sound like:
1. _Visit: `https://example.cypress.io`_
2. _Find the element with content: `type`_
3. _Click on it_
4. _Get the URL_
5. _Assert it includes: `/commands/actions`_
6. _Get the input with the `action-email` data-testid_
7. _Type `fake@email.com` into the input_
8. _Assert the input reflects the new value_

## Testing your own app
To begin writing tests for your own app you need to run the dev server, and point the tests there using the same syntax that we used to point the example test at the Cypress example site:
```typescript
describe('The Home Page', () => {
  it('successfully loads', () => {
    cy.visit('http://localhost:8080') // change URL to match your dev URL
  })
})
```
Because you are going to be visiting this URL in every test, it is a good idea to add it to the config file as the base URL, which will allow relative URLs to be used in visit statements (ie "/" instead of "http://localhost:3000/"). Do this by adding the following to your config file:
```typescript
import { defineConfig } from 'cypress'

export default defineConfig({
  e2e: {
    baseUrl: 'http://localhost:8080',
  },
})
```


