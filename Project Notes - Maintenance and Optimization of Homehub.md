#projects 

Now that homehub is hosted and ready for use testing and further development, this doc will serve as a record of the things that I learn while maintaining and optimizing the site. 

## 08/06/2023
Current issues to track down:
- [ ] Next auth session callbacks failing due to failed GET endpoint request. Likely due to accessing the database in the callback to fetch userId. Look at the callback in the taxonomy git repos source code and see if you can pull the sessionUser Id from the token instead.
- [ ] Google disallowed user agent issue. Only happend once and have not been able to reproduce. May be tied to the session callback issue but I am not sure. May require more devices/google accounts to test for this

## 09/06/2023
Todays tasks: 
- Look into both issues identified yesterday and record any findings here
- Get comfortable working on branches in Git so that the site can be developed seperate from the production build on main branch
- Continue active development, starting with the todo route

### Active dev on the todo route
The first task for today and my first application of Git branching will be to fix the non-responsive todolist display. Currently the buttons for toggling status and deleting tasks on the todo page are too big and look awkward on mobile screens. I intend to fix this by having them display as icons instead of buttons at mobile screen sizes. 