#general #testing

Sources used so far for these notes are: [this blog](https://moduscreate.com/blog/an-overview-of-unit-integration-and-e2e-testing/), [webdev cody video](https://www.youtube.com/watch?v=PzrhclEQp-M), [Martin Fowler article](https://martinfowler.com/articles/practical-test-pyramid.html),  [Kent Dodds article on the testing trophy](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications).

## Automated testing for development
The simplest approach to testing a software product and the one almost everyone starts with is manual testing. Once you have debugged the code and there are no errors beign thrown, this is as simple as navigating through the product and checking manually if everything behaves the way you expect or designed it to do so.

This is a good thing to do. It is always helpful the be very familiar with the product and in particular the user experience. However, it is inefficient to be doing this constantly, especially if you are pushing many small changes or micro features in a day, and this is where automated tests fit into the dev workflow. Automated tests can be set up to fire everytime you commt new code or merge a branch into main, helping you easily catch issues, breaking changes or side effects.

## Types of test
There are four main types of test that make up a solid modern testing regime in software dev. From  least to most complex these are:
1. <span style="color: cyan; font-weight: bold;">Static tests:</span> these are testing methods that do not require the code to be run to return there check value. Tools like eslint or the static typechecking of TypeScript are examples of static testing that I use in existing projects
2. <span style="color: cyan; font-weight: bold;">Unit tests:</span> Simple direct tests that operate on a single function or other smallest possible unit of code and test it's output on multiple test cases against the expected output recorded by the test writer. These do not consider dependancies between units of code or any complex interactions
3. <span style="color: cyan; font-weight: bold;">Integration tests:</span>These are tests that probe the interactions between modules. They are both more comprehensive and more complex than unit tests
4. <span style="color: cyan; font-weight: bold;">End to end tests:</span> These simulate specific user action flows, for example clicking submit on a form, and look for expected vs actual behavior.
It is worth noting here that when working with component based libraries like React, the line between unit tests and integration tests on the front-end can get pretty blurry.

## Testing philosophy
The traditional, or at least more established testing philosophy is the testing pyramid, popularised by Martin Fowler:
![[Pasted image 20230728081026.png]]
The basic principle here is that we should group software tests into buckets of different granularity, and the pyramid shape gives us an idea of roughly how much testing should be done with each of the categories of tests. Note that the pyramid does not inculde static tests. 

A slightly tweaked but more modern version of this philosophy, wriiten about by Kent C. Dodds is the testing trophy pattern:
![[Pasted image 20230728084806.png]]
This includes static tests and linting as the base upon which everything rests, and puts more focus on integration tests. The concept was born out of the very simple idea that you should write tests, not too many, mostly integration tests. Kent also clarifies a little how he differentiates between unit and integration tests:
<blockquote style="font-weight: bold; font-style: italic; color: cyan;">Unit tests are those which test units which either have no dependencies (collaborators) or which have those mocked for the test. Integration tests are those which test multiple units integrating with one another.</blockquote>

## Testing tools
Depending on the language or possibly the specific application, you will use different libraries and tools for testing. Here are links to the ones I have researched so far:
- [[JavaScript - E2E and Component Testing with Cypress]]