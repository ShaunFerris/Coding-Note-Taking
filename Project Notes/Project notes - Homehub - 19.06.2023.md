#projects 

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
- [x] Wiring up the consolidated components to interact with the database through context
- [x] Adding delete functionality to the main budget list page


## Next entry [[Project notes - Homehub - 20.06.2023]]