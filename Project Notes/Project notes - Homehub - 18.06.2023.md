#projects 

## 18/06/2023
Did a little cosmetic work on the budget select/add page yesterday, nothing too major. Also noticed yesterday that the list of bugets on this view is not properly updating when a change is made to the database. Wierdly, it is behaving as expected/wanted on the dev server, but not on the actual live version. I tried a few different things yesterday to try and fix this including experimenting with the cache: no-store and revalidation options but could not get it working correctly.

I decided today to pull all of the actual functionality out of the page.js file and into a child client side component, hoping that that would fix it, as I can't see any reason that it won't simply behave like the todo list and shopping list fetch logic, which is written almost the same exact way and works. Maybe it has something to with the fact that those routes use context to trigger the use effect? Anyway pulling it into a component also did not fix the issue.

Next I tried adding the req argument to the get request in the API endpoint, because excluding it has caused me headaches in the past even when it does not appear to be required. This also did not work, AHHHHH. I guess on Monday I will dive into this more deeply, but I'm seriously running out of ideas for why it won't work the way I expect, like the TODO and shoplist routes that do a very similar thing.

### NOTE FOR TOMORROW
Looking again at the difference between the budget list and the shoplist, the only difference that I can see that might be responsible is that the shoplist passes the fetched data to a component that renders the the actual list item, whereas on the budget list I am just using list items mapped to the data as part of the components return. Tomorrow I will make a new component for the list items and see if that fixes it, becusae maybe when the list of budgets changes, it will trigger a re-render of the child components that are passed info from that list as props?

## Next entry [[Project notes - Homehub - 19.06.2023]]