#projects 

## 12/06/2023
Spent a frustrating amount of time looking at a site footer today, ended up just going for a simple one and only having it display on the main page, as including it in the layout made it a nightmare to manage the spacing around the different elements on each page at each screen size.

Made a dev branch for the process of rewriting the todo api routes and experimenting with making the server components async so that they don't render untill the data has been fetched. This may take some time and experimentation so I wanted to be able to regularly commit my work without interrupting service of the site.

Most of todays experiments were failures and I ended up abandoning the dev branch. It seems that the interactivity I need between my data and the user makes server components kind of incompatible with this project. So for now I will keep the current system where in the page comonents for the todos and shoplist routes, and by inheritance their children, are client components. I may experiment with server side routing a little more with the budget route when I get on to that. However given that hooks are incompatible with server components and that there does not seem to be a way to refresh data in server components in response to an action, I'm not sure if this is reconcilable.

## Next entry [[Project notes - Homehub - 15.06.2023]]