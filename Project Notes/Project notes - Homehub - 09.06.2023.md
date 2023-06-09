#projects 

## 09/06/2023
Todays tasks: 
- Look into both issues identified yesterday and record any findings here
- Get comfortable working on branches in Git so that the site can be developed seperate from the production build on main branch
- Continue active development, starting with the todo route

### Issue investigation
First I tackled the issue identified yesterday where session callback GET requests were frequently failing. It appears to be fixed but keep in mind that may not be true, further testing will confirm for sure. 

Here is my current understanding of the problem, and an explaination of what seemingly fixed it:
- Previously, the session callback was querying the database to get the `_id` of the user, and make it available through the `getSession()` function, but was not awaiting `connectToDb()` first. This meant that sometimes the  database was not connected, so the session callback would fail. This caused the user to be signed in, but logic that checked for a session would not fire so protected routes would be locked and the main menu would never appear unless the page was refreshed. 
- The `connectToDb()` function **_could_** be called here, but that is not reccomended as it would hurt performance to reconnect everytime session needs to be checked, and turned out to not be neccessary.
- The jwt call back is an optional callback for the nextAuth options object that is triggered by requests to `/api/auth/signin`, `/api/auth/session` and calls to `getSession()`, `getServerSession()`, `useSession()`.
- The arguments _user_, _account_, _profile_ and _isNewUser_ are only passed the first time this callback is called on a new session, after the user signs in. The token arg is always available.
- Because the jwt callback is always called immediately on sign in, the db should always be connected when it is first called, so I made the database query for the current user here and added it to the token, then I access the token in the session callback to pass the id to session where it can be made available to use in the code.

### Active dev on the todo route
The first task for today and my first application of Git branching will be to fix the non-responsive todolist display. Currently the buttons for toggling status and deleting tasks on the todo page are too big and look awkward on mobile screens. I fixed this by adding a div containing icons with the requisite functions as onClick callbacks at the same depth as the div containing the buttons, and using tailwind CSS breakpoints to display one or the other based on mediaQuery.

### Side quest
Decided to investigate whether I could make the homepage/login experience a bit better by moving the conditional display of the card menu/login card to the page file itself.

This turned out not to be possible becuase the condition for displaying the logincard or the cardmenu is the existence of a session token, and you can't use the getSession hook at the top level. What I did instead was to destructure the status as well as the data from the get session call in the loginCard component. Then I added a new condition to the ternary statement that renders the card/cardmenu which checks if status === "loading", and if so renders an animated loading spinner. It works great and really improved UX.

### Tasks for next session
Things to look at next time I work on the project:
- Start re-writing the data fetching and routing for the todolist route. In particular, investigate making the components that do the fetching async server components. I think this will make it so that the component is not fully mounted until the data is fetched, which will allow the proper use of suspense and the Loading.js special page to improve UX. I can then experiement with wether it is better to revalidate the data, or continue using a context to track db changes and useEffect to re-fetch.
- Put a footer component in the top level layout with my name and a github link
- Start seriously thinking about the budget and profile routes

Point one is the most important and interesting one but also the largest amount of work. Don't forget to look at the data fetching docs again and really try to figure out suspense/data streaming/caching.

## Next entry [[Project notes - Homehub - 12.06.2023]]