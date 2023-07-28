#projects #testing

This note will keep my dev diary entries for the process of backfilling a suite of tests for the existing code on homehub. I have decided to prioritise this over working on the other new features for a start, as similar to the typescript adoption I want to get comfortable with it so that all the new feature code I write can be in <span style="color: cyan; font-weight: bold; font-style: italic;">TypeScript</span> and have <span style="color: cyan; font-weight: bold; font-style: italic;">test coverage</span> as it is produced.

To this end I will be using Cypress to build my testing suite, starting out with the high-level E2E tests, then integration and unit tests.

## Status 28.07.2023
Today I just got to grips with the basics of the Cypress library and implemented some basic tests for the home page route that check the existence of the content/titles etc and follow the links in the footer to makes sure they point to the correct place.