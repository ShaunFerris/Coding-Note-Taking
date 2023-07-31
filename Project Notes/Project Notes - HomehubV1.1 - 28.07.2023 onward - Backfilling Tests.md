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

