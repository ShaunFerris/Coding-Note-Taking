#projects 

## 16/06/2023
Yesterday I set up a rudimentary page for displaying a list of budgets and adding a new one, which then navigates to a dynamically routed budget page for that given budget.

Today I will begin by tidying up the budget menu page so that it look reasonable and tidy. Then I will look into refactoring the budget componenet structure so that the componenets can recieve the budget data fetched at the dynamic page level. This is because currently there are too many components and the share data through context but don't get real data from anywhere yet.

### Tidying up the new budget main page
Before getting too deep into making this page look nice I immediately got distracted by the fact that adding a new budget does not immediately navigate you too that, which it should. I dove into using the use router hook to do this. Then I ran into trouble figuring out where to get the id for the newly created Budget db entry from, before realising it is included in the response, so I should be able to pull it out of there as part of the data fetching function that makes the POST request.

I messed around for a while trying tho have the routing happen on click of the button but that seemed to never work, because the onclick event always fires before the fetch has finished and the id is set. Then I tried having the routing happen after the response is received in the fetching function, but that is not working either. That's where I'm currently at and this is not solved yet.

## Next entry [[Project notes - Homehub - 18.06.2023]]