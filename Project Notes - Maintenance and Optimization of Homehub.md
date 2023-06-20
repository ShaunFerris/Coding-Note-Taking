#projects 

Now that homehub is hosted and ready for use testing and further development, this doc will serve as a record of the things that I learn while maintaining and optimizing the site. 

## 08/06/2023
Current issues to track down:
- [x] Next auth session callbacks failing due to failed GET endpoint request. Likely due to accessing the database in the callback to fetch userId. Look at the callback in the taxonomy git repos source code and see if you can pull the sessionUser Id from the token instead.
- [x] OAuth disallowed user agent issue. Only happend once and have not been able to reproduce. May be tied to the session callback issue but I am not sure. May require more devices/google accounts to test for this

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

## 12/06/2023
Spent a frustrating amount of time looking at a site footer today, ended up just going for a simple one and only having it display on the main page, as including it in the layout made it a nightmare to manage the spacing around the different elements on each page at each screen size.

Made a dev branch for the process of rewriting the todo api routes and experimenting with making the server components async so that they don't render untill the data has been fetched. This may take some time and experimentation so I wanted to be able to regularly commit my work without interrupting service of the site.

Most of todays experiments were failures and I ended up abandoning the dev branch. It seems that the interactivity I need between my data and the user makes server components kind of incompatible with this project. So for now I will keep the current system where in the page comonents for the todos and shoplist routes, and by inheritance their children, are client components. I may experiment with server side routing a little more with the budget route when I get on to that. However given that hooks are incompatible with server components and that there does not seem to be a way to refresh data in server components in response to an action, I'm not sure if this is reconcilable.

## 15/06/2023
Last couple days did not do too much work on the site, just a few minor cosmetic fixes and adjustments. Going to look at properly rewriting the todo data fetching logic today to remove the unneccessary reducer component in the context.

Actually, does it need to be rewritten? There are two different list components, the pending and the completed lists, and they both need to have the same version of the data. If I move the data fetching outside of the context, won't that mean that every data fetch is happening twice?

Hmmmmmm. I could fetch the data in the page component and pass it as props to the lists maybe?

I think it's best to keep doing the GET request data fetching inside the context file, so that it doesn't happen redundantly in both lists, and pass the data to the lists through context. However, I think I can remove the reducer functionality, because currently we literally never use dispatch to send data back to the contect. I think we can just set the inital state with useState, fetch the data in a useEffect and pass it down through the provider.

This turned out to be simple, future testing will determine if I gained any performance from this change. It still bothers me that I can't see a way to utilise client side data fetching when I require this level of interactivity, but maybe that will become clear as I continue to learn more.

### Goals moving forward
The most important thing to tackle next is getting the budget route up and running in some form. The specifics aren't important as they can be changed later, just get it connected to the database and working in terms of persisting the data.

After that the next most important thing is probably properly securing the api routes, probably with useSession I guess? Need to look into that more because I'm not sure use session can even be used on the server side.

### Continuing on the budget route
I have decided to make it possible to have multiple seperate budgets for different periods. This has required me to move the exisitng budget page.js file into a new dynamic route that will be identified by it's ID, and build a new page.js that fetches existing budget data from the db and lists them, then routes you to a dynamic page when you select one. 

Currently I have complete a rudimentary  page component that fetches a list of budgets from the database and shows them in a list, and below that give the ability to add a fresh budget with a chosen name and starting amount to the database. If you click on a budget from the list, you are routed to a dynamic page for that budget, where the url slug is the `_id` from that budgets database entry. Next I need to change the dynamic budget page so that it can pull the name, budget and expenses list from the database by getting the id from it's own dynamic URL slug.

## 16/06/2023
Yesterday I set up a rudimentary page for displaying a list of budgets and adding a new one, which then navigates to a dynamically routed budget page for that given budget.

Today I will begin by tidying up the budget menu page so that it look reasonable and tidy. Then I will look into refactoring the budget componenet structure so that the componenets can recieve the budget data fetched at the dynamic page level. This is because currently there are too many components and the share data through context but don't get real data from anywhere yet.

### Tidying up the new budget main page
Before getting too deep into making this page look nice I immediately got distracted by the fact that adding a new budget does not immediately navigate you too that, which it should. I dove into using the use router hook to do this. Then I ran into trouble figuring out where to get the id for the newly created Budget db entry from, before realising it is included in the response, so I should be able to pull it out of there as part of the data fetching function that makes the POST request.

I messed around for a while trying tho have the routing happen on click of the button but that seemed to never work, because the onclick event always fires before the fetch has finished and the id is set. Then I tried having the routing happen after the response is received in the fetching function, but that is not working either. That's where I'm currently at and this is not solved yet.

## 18/06/2023
Did a little cosmetic work on the budget select/add page yesterday, nothing too major. Also noticed yesterday that the list of bugets on this view is not properly updating when a change is made to the database. Wierdly, it is behaving as expected/wanted on the dev server, but not on the actual live version. I tried a few different things yesterday to try and fix this including experimenting with the cache: no-store and revalidation options but could not get it working correctly.

I decided today to pull all of the actual functionality out of the page.js file and into a child client side component, hoping that that would fix it, as I can't see any reason that it won't simply behave like the todo list and shopping list fetch logic, which is written almost the same exact way and works. Maybe it has something to with the fact that those routes use context to trigger the use effect? Anyway pulling it into a component also did not fix the issue.

Next I tried adding the req argument to the get request in the API endpoint, because excluding it has caused me headaches in the past even when it does not appear to be required. This also did not work, AHHHHH. I guess on Monday I will dive into this more deeply, but I'm seriously running out of ideas for why it won't work the way I expect, like the TODO and shoplist routes that do a very similar thing.

### NOTE FOR TOMORROW
Looking again at the difference between the budget list and the shoplist, the only difference that I can see that might be responsible is that the shoplist passes the fetched data to a component that renders the the actual list item, whereas on the budget list I am just using list items mapped to the data as part of the components return. Tomorrow I will make a new component for the list items and see if that fixes it, becusae maybe when the list of budgets changes, it will trigger a re-render of the child components that are passed info from that list as props?

## 19/06/2023
So I am likely still going to try extracting the display of the budget list items into a component like I noted down yesterday, but I'm not at all sure that this is the solution to the problem.

I think the problem is being caused because the fetch request to GET endpoint is being cached, and identical requests are served the same result from the cache , even if the database has changed. However I don't know why this caching is happening for this endpoint, but not for any of the others, I can't see any differences in the code that might cause it. Also, using the cache: no-store or revalidate options on the fetch does not seem to solve the problem either.

In addition to this, when I look at the build summaries for the site on vercel, all the api routes are functions EXCEPT for api/budget/route.js, the problematic endpoint, which is showing as an ISR function. This would explain why the data fetch only ever properly reflects the database immediately after a build, but not why I can't seem to get it to revalidate.

It appeard that the revalidate and no-cache options are just broken? I know this is supposed to be a stable relesase but they really seem to not work. I thought about updating my next but I fear that will break the project. 

Found a stackoverflow entry about a workaround where accessing the request in the endpoint will cause it to work. If this works for me then I guess that points to the issue being with next.js request de-duping, and becuase the content of the GET request is always the same it keeps treating it as a duplicate request and returning old data. That would still imply that the no-cache and cache revalidation options are broken though.

<h1 style="text-align: center;">IT WORKED</h1>
So it was most likely a de-duping issue! Simply console logging the request.url from inside the endpoint code fixed this issue.

It's also worth noting that after adding this console.log to the endpoint, the endpoint is now being built as a function and not an ISR function by vercel, so that difference was part of the problem, and the revalidation/cache: no-store options that should have made the ISR version work dynamically WERE BROKEN.

### Moving forward from this annoying ass bug
Next things to look at are:
- [x] Re-writing/re-organizing the component structure of the dynamic budget page routes. There are too many fucking components rn, so I will consolidate some of them in a way that makes more sense.
- [ ] Wiring up the consolidated components to interact with the database through context
- [x] Adding delete functionality to the main budget list page

## 20/06/2023
Started off today by working on the component structure of the budget dynamic route. I have successfully combined the three alert components that were displaying the budget amount, spent so far and remaining values into one component that takes the budget data as props and displays the values.

Next I started working on functionality to add an expense to the budget, and this is were I ran into a bunch of problems. I had been trying to pass the data fetched in the page components down to all the other components but neglected that if those components change the data, I can't throw it back upstream. This is a text book situation where it makes sense to use context. So I am going to next move the data fetching for the current budget out of the dynamic budget page and into the context, and use the reducer strategy that the non-persistent version of the route had, but just have the context make fetches to the API rather than it's own state.