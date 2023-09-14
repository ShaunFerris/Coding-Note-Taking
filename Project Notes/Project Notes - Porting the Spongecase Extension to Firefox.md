#projects 

## Why was a hard port necessary?
Browser extensions use browser apis to do things like set event listeners and execute scripting actions. Chrome uses the chrome object namespace for these while fire fox uses the browser namespace. These are all essentially the same apis, eg `chrome.notifications.create()` works exactly the same way as `browser.notifications.create()`. In fact firefox apparently even works with the chrome namespace in-place of browser. So why did I need to port this extension? 

<span style="color: cyan; font-weight: bold; font-style: italic;">The extension was written with the manifest V3 standard, and chrome  and firefox have different specifications for background scripts within manifest v3.</span>

Details can be found in this very helpful stackoverflow thread: https://stackoverflow.com/questions/75043889/manifest-v3-background-scripts-service-worker-on-firefox

This may change in the future but for now I wanted to get a firefox version working so that I could use my dumb little tool personally as I don't really use chrome.

I also decided along the way that I would just port it over to the browser object namespace since I was already tinkering around, and remove a few superfluous things/try to optimize if I could. 