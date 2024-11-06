#webdevSpecific 

Part three and four of the FSO content have focused heavily on the back end. The example front-ends from part 2 do not yet support the user management and auth funcitonalities we have practiced implementing on the back end. 

Thinking about the example notes app, the front-end can currently show existing notes and let users change the notes importance property. New notes cannot be added anymore because they are not submitted with a token, and the backend now expects a token to identify the user before it will complete the POST operation. 

We will now look at implementing user management and auth in the front end of the notes app example, with exercises doing it in the blog app example for real. Lets start with handling logins on the front end.

## Handling login
