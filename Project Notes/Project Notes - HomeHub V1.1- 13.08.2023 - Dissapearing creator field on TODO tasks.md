#projects 

I thought this was a serious problem that would need solving, but now it is looking like a very simple issue that did not even make it to prod, which is what I thought happened. I will still document the issue here though as it was a useful learning exercise to find, id and hotfix this issue.

## What happened?
For future ref, this bug happened on this commit: `6196487c43232eb10ba280815b39b134e2c78359` and was fixed on this commit: `f414cccee9797f6fbae078d50dd67cf11ace99e8`

I was working on the conversion of the todo route from JS to TS. The conversion was very straightforward for the most part, with none of the componenets or the page file requiring any additions, and only the context file needing a new interface. This was very much inline with my experience converting the shopping list route which makes sense as the two routes are very similar. This all went fine and the page loaded without errors so i pushed without doing any proper manual tests. Bear in mind that at this point I have written tests for the homepage and shoplist routes, but have not got to the todos yet, so I had no automated tests to run.

After pushing the build live, I did some manual testing, and the route crashed when a new task was added. The error message pointed out that the `creator` field of the task object was null, which caused the front end code that renders the task item in the list to crash out. I initially thought that this was down to some typescript issue, despite the fact that no TS problems were shown in the IDE. This was somewhat logical because that was all that I had changed since the last time it worked: except that it wasn't, I had forgotten about migrating to the new db for testing yesterday. Anyway I attempted to fix it by strongly typing the todo items in the interface for the todocontext. This of course didn't fix it but was a good thing to do anyway.

I then hotfixed the problem by using conditional chaining in the code for displaying the tasks creator:
```typescript
//Original
<span className="text-gray-500">
     | {task.creator.username}
</span>

//Fixed
<span className="text-gray-500">
     | {task?.creator?.username}
</span>
```
This fixed the issue, rendering an empty field when the task creator field was null.

## Investigation and the real problem
The hotfix above skirts around the real issue but was also the key to figuring it out. As I mentioned above, the issue turned out to be related to yesterdays migration to the second database instance for testing. I checked the db entry for the testing task and confirmed that the creator field was not in fact null, it contained a mongodb id string as expected. However, in the testing database unlike the prod one, the users collection is empty as I have not gone through the process of logging in in the staging environment. My session is being confirmed by the token leftover from before the db migration. I might still leave the conditional chaining syntax in place as a pressure valve incase some other issue ever causes the creator field to be null, as having the creator field blank on the UI is better than crashing out.

I went throught the process of signing out and back in on the testing environment, but of course I have a check in place that doesn't allow signin if your account is not already in the db. I copied the entry for my user account over from the prod db to the testing one and then was able to sign in, and the creator field was no longer null on the todo route, bug fixed.