#projects 

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

## Next entry [[Project notes - Homehub - 16.06.2023]]