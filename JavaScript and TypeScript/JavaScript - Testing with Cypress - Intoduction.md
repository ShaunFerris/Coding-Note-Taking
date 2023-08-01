#javascript 

Cypress is a testing solution for web applications, that purports to make test driven development fun and easy for JS devs. It is well established that consistent testing with good coverage is the right thing to do, it is industry standard for a very good reason. But in front-end JS project is often complicated, slow and difficult. 
<blockquote style="font-weight: bold; font-style: italic; color: cyan;">Cypress provides an open source, browser based test runner that can experience your site just like an end user would. </blockquote>
Cypress may be set up to fill out a form, click the submit button and then navigate to the user dashboard page. All of these actions will be done programmatically based on code defined by the test writer, and every test is recorded along with a snapshot at every step, making it possible to quickly time travel through the UX and find out exactly which parts of the code are responsible for off target behaviour. 
<blockquote style="font-weight: bold; font-style: italic; color: cyan;">In addition to this excellent E2E functionality, Cypress is also great for integration testing and unit testing. It is a total solution for testing the front-end of moder, complex JS apps.</blockquote>

## Getting started
Install Cypress as a development only dependancy in your project directory:
```bash
npm install cypress --save-dev
```
Then run:
```bash
npx cypress open
```
This command will open up the Cypress lauch pad, presenting you with the option of configuring either end to end or component testing. Alternatively, if you're not sure which type you want and just want to get on with your testing journey, just choose **E2E** for now - you can always change this later!

On the next step, the Launchpad will scaffold out a set of configuration files appropriate to your chosen testing type, and the changes will be listed for you to review.

## Details of Cypress testing
For a more in-depth guide on E2E testing with Cypress see: [[JavaScript - Testing with Cypress - E2E Tests]]

For a guide to components testing with Cypress see: [[JavaScript - Testing with Cypress - Component Tests]]

For more specific sub topics of testing with Cypress see:
- [[JavaScript - Testing with Cypress - Stubbing and Spying]]