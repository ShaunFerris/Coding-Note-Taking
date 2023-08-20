#projects 

## Feature planning - 18/07/2023
This note is for the planning and brainstorming of features for the first major update to my home organization app HomeHub. This doc will outline the plans, and dev diary entries for work towards v1.1 will be linked at the bottom.

<blockquote style="color: cyan; font-weight: bold; font-style: italic;">The objective of this update is to identify and implement in HomeHub, features that will show a level of complexity and poilsh that will set it apart from a generic todo/budget/shopping list app and make it more technically impressive to prospective future employers.</blockquote>

The current plan is to work on new major features and keep the files in the repo, but unrouted until they are tested and ready. This is maybe a little hacky, but I don't really need to use git branching as the only person working on this so I think just commiting un-routed code to main will be fine and keep it nice and tidy for me while developing.

### Minor issues from current version that need addressing:
- [x] DELETE route for the budgets list/budget components
- [x] Clear list functionality for the todo route so we can mass delete
- [ ] Standardising of layouts for all components that display a list of some kind

### Meta feature #1 - Typescript
Not really a feature per-se, but I want to convert the app to typescript over time. I need to learn it as it is becoming clear to me that it is an essential skill for modern web-dev. Rather than trying to convert the whole app in one go, especially since I don't know it yet, I will be adopting the strategy of writing new features/components in TS so that I can test and learn before integrating them into the actual build. After I feel like I have a good handle on it, I will then re-write the remaining original code to be typesafe.

### Meta Feature #2 - Tests
This is another feature that will impress potential portfolio viewers. Industry development environments always implement testing suites and if they don't then they should. I have no experience with this yet and it is a weak spot for me so this will help. Also if I do get more users once the walled garden opens the app up to use by more people, it will also be genuinely useful.

I am going to be using Cypress to build the testing suite, and will likely start by implementing E2E testing for UX on all the routes, then move on to Component Testing for unit and integration test coverage. 

See the dev diary entry linked at the bottom of this note for current progress.

### Feature priority #1 - Walled garden architecture
**This is the most important and impressive feature that I have been thinking about implementing so far.** 

Currently the site is set up entirely for the initial intended audience: myself and members of my household. The app is hosted live on a publicly accessible site, but only whitelisted google user credentials are able to successfully log in, and without login auth no routes outside the home page will render for the client.

This system works great within the original scope of the app, which was to perform simple task tracking for my household in a way that persisted the data, kept it current for all users, and had a nice UI that I could tweak overtime as we used the app. However the problem is that I would also like to use this project as a portfolio item, as it is by far my biggest and most impressive project to date. Any prospective employers could read the source code, or I could put together a document in the readme that details all the features, but this a fairly big barrier to expect recruiters to get over. <span style="color: cyan;">Implementing a walled garden architecture where ANY user can sign up, create a home group that other members can join, and then have their own database instance would both allow recruiters to easily check out the UI/UX, and would constitute a more impresive app - Win/Win.</span>

A general outline of what I think needs to be done to get this working:
- New database schemas: Need a schema for user groups, and a new schema for users that ties them to a user group. Maybe every schema should have a user-group entry? This way I wouldn't need to spin up a new todo/budget/etc db instance for every user group, but rather store all the documents together and just pull by the user group of the logged in user
- Bulid the user profile route properly so that users have a place to manage their user groups. This idea actually gives the user proile route a reason to exist which is nice. **There is a dev diary entry for this work below**.

## V1.1 Dev diaries
- [[Project Notes - HomehubV1.1 - 20.07.2023 - tsconfig Issue]]
- [[Project Notes - HomehubV1.1 - 21.07.2023 onward - Profile Route]]
- [[Project Notes - HomehubV1.1 - 28.07.2023 onward - Backfilling Tests]]
- [[Project Notes - HomeHub V1.1- 13.08.2023 - Dissapearing creator field on TODO tasks]]