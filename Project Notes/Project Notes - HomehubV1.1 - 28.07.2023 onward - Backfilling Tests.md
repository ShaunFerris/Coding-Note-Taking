#projects #testing

This note will keep my dev diary entries for the process of backfilling a suite of tests for the existing code on homehub. I have decided to prioritise this over working on the other new features for a start, as similar to the typescript adoption I want to get comfortable with it so that all the new feature code I write can be in <span style="color: cyan; font-weight: bold; font-style: italic;">TypeScript</span> and have <span style="color: cyan; font-weight: bold; font-style: italic;">test coverage</span> as it is produced.

To this end I will be using Cypress to build my testing suite, starting out with the high-level E2E tests, then integration and unit tests.

## Status 28.07.2023
Today I just got to grips with the basics of the Cypress library and implemented some basic tests for the home page route that check the existence of the content/titles etc and follow the links in the footer to makes sure they point to the correct place.

## 31.07.2023
Picking up where I left off on Friday, looking to finish building out end to end tests for the home page then extending out to the other routes. May look at testing the google authentication today as well. Try to get a battery of tests and and going without losing focus on the new feature work, or on the other studying I need to do.

The first thing to actually do today is to go back over the tests that I wrote on Friday and re-do them to use the `data-test` attribute to grab elements from the DOM for testing, as this is best practice and will help future proof the tests for changes to the site structure.

Learnt a new syntax while doing content display tests for the homepage that lets you chain together assertions. For example I wanted to make sure that the homepage footer would display the text "Built by Shaun Ferris" and "Source code available on Github" but these two strings are in different sub elements of the main footer, so you can't just test that the footer displays them as one string. I did it like this:
```typescript
cy.get("[data-test='homepage-footer']")
	.should("contain", "Built by Shaun Ferris")
	.and("contain", "Source code available on Github.");
```

It occurred to me that I need to be able to test the loginflow, and that I need to be able to mockup a logged in state in order to test basically every page properly. This turned out to be a non-trivial task. First priority is to figure out how to mock up a logged in state so we can test the whole site from the perspective of a logged in user.

**Some useful resources I looked at when figuring out tests for the login flow:**
- [Good blog post](https://filiphric.com/use-session-instead-of-login-page-object-in-cypress)
- [The next auth example](https://next-auth.js.org/tutorials/testing-with-cypress)
- [A github thread, look for the post about mocking a user session token](https://github.com/nextauthjs/next-auth/discussions/2053)
- [Pull request where someone is implementing the mocked user method from the above thread, but with useful updates](https://github.com/scientist-softserv/webstore/pull/197/files#diff-1b1a73ba561eab6738c8b62510feed5b10edcd25c56626ac79017752b806439d)

Ended up using the method from the pull request above to implement user session mocking and allow me to test the pages requiring auth. [This resource](https://www.howtocode.io/posts/cypress/cypress-environment-variables) was also useful for understanding how the pull request in question got the valid token value from the hidden and uncommited .env into the cypress config's env section for use in the `Cypress.login` custom function.

## 01.08.2023
After successfully mocking the user session token yesterday, today I am continuing to write tests. The tests for the authenticated/un-authenticated versions of the card menu worked flawlessly before I pushed yesterday, but that turns out to not be reproducible. The amount of time that the loader hangs around is too unpredictable. Today I am starting out by looking into using the a package that extends Cypress with a [wait-until](https://www.npmjs.com/package/cypress-wait-until) command. This would allow me to tell it to wait until the loading spinner is gone before testing the content of the card menu. 

Ended up having a fair amount of trouble with this. I initially thought I was just having issues with the wait-until package, but it turned out to  be more of a fundamental issue. I was writing assertions that relied on the wrong elements, i.e. I was asserting the content of the card menu in the logged in/out state when that is just a wrapper for the conditionally displayed components. I ended up fixing it by asserting about the conditional components themselves, not the wrapper. 

At this point I removed the wait-until lib as a dependancy, as the homepage tests are now working withough using it.

**At this point I discover that the tests don't actually pass everytime**.

Seems likely to be down to the same issue as earlier, the test is timing out before the loading spinner is replaced with the conditional component. May need to look into calling session and waiting on it before running the test.

**Fixed this issue** by intercepting the call to the session endpoint of the nextauth api and forcing a wait for it's return, the isolated stubbing tests look like this now:
```typescript
describe("Loads login prompt when unauthenticated", () => {
  it("Visits the page and stubs logout", () => {
    cy.logOut();
    cy.visit("/");
    cy.intercept("GET", "/api/auth/session").as("session");
    cy.wait("@session").then(() => {
      cy.get("[data-test='login-prompt']");
    });
  });

  it("loads the menu when authenticated", () => {
    cy.clearCookie("next-auth.session-token");
    cy.login(process.env.TEST_USER, process.env.TEST_PASS);
    cy.visit("/");
    cy.intercept("GET", "/api/auth/session").as("session");
    cy.wait("@session").then(() => {
      cy.get("[data-test='card-menu']");
    });
  });
});
```
The resource that really helped me to crack this problem was [this](https://webtips.dev/webtips/cypress/wait-for-element-to-disappear) blogpost.

## 04.08.2023
Have worked on this for the last few days with no major hurdles. Finished out tests for the homepage and started working on the shoppinglist route. I noticed that I was often repeating lines of code waiting for data resources to load before checking things that need the data to render, so alias'd that behaviour out to this custom command:
```typescript
Cypress.Commands.add("waitForData", (time: number) => {
  cy.intercept("GET", "/api/**").as("pageload");
  cy.wait("@pageload", { timeout: time });
});
```

Similarly I put the code for navigating around next chunk errors into a reusable command. <span style="color: cyan; font-weight: bold; font-style: italic;">Remember that this is a workaround until I figure out and eliminate these webpack errors. This is only acceptable as they don't affect UX, they just cause all components to render client side instead of having some on the server.</span>
```typescript
Cypress.Commands.add("logChunkError", () => {
  Cypress.on("uncaught:exception", (err) => {
    console.log("Cypress detected uncaught exception: ", err);
    return false;
  });
});
```

Planning to also extract out commands for waiting for the session token to come in and be checked.

## 12.08.2023
Back to work today on the end to end testing suite. Focussing for now on finishing the shopping list route tests, then complete similar e2e tests for the other routes. At this point, I think it will be a good time to return to working on the new content, likely starting with the profile route: [[Project Notes - HomehubV1.1 - 21.07.2023 onward - Profile Route]]

**Side Note:** Also noticing today that there is some overall slopiness in the codebase still that could do with cleaning up. A good place to start would  be the console logs that are still in place on some of the API routes. I have better tech for debugging now and it looks sloppy.

Another thing to look at today or tomorrow is setting up a staging database instance that the local host version of the app uses, so that e2e tests don't fuck with the actual database entries evey time I want to run tests. Managed this without too much trouble in the end, the process was:
1. login to atlas, go to my cluster and create a new database. Note that there are tight restrictions on naming for mongodb instances, ie no spaces allowed
2. add the new testing databases name to the URI in the local env file like this: `...mongodb.net/your_db_name?retryWrites...` 
3. update the env variable on vercel so that it includes the prod db name in the same place. 
Done!

