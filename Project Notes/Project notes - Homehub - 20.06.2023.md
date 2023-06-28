#projects 

## 20/06/2023
Started off today by working on the component structure of the budget dynamic route. I have successfully combined the three alert components that were displaying the budget amount, spent so far and remaining values into one component that takes the budget data as props and displays the values.

Next I started working on functionality to add an expense to the budget, and this is were I ran into a bunch of problems. I had been trying to pass the data fetched in the page components down to all the other components but neglected that if those components change the data, I can't throw it back upstream. This is a text book situation where it makes sense to use context. So I am going to next move the data fetching for the current budget out of the dynamic budget page and into the context, and use the reducer strategy that the non-persistent version of the route had, but just have the context make fetches to the API rather than it's own state.

Did a rework of the data fetching so that is is done in a use effect with NO dependancies inside the context provider, and made available along with the dispatch function so that the add expense form component can modify the shared budget state.

Next, I need to build a patch endpoint. The current set up has the user able to change every aspect of the budget data model except the name, on this one page. The components on the page share the current state of the budget data model through context, and when they operate they operate on the context itself, not the actual data on the db.  I think this is good, because it makes adding expenses or modifying the budget amount really snappy and independant on the network, but it's impermanent without the PATCH route.