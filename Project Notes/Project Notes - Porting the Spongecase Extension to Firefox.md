#projects 

## Why was a hard port necessary?
Browser extensions use browser apis to do things like set event listeners and execute scripting actions. Chrome uses the chrome object namespace for these while fire fox uses the browser namespace. These are all essentially the same apis, eg `chrome.notifications.create()` works exactly the same way as `browser.notifications.create()`. In fact firefox apparently even works with the chrome namespace in-place of browser. So why did I need to port this extension? 

<span style="color: cyan; font-weight: bold; font-style: italic;">The extension was written with the manifest V3 standard, and chrome  and firefox have different specifications for background scripts within manifest v3.</span>

Details can be found in this very helpful stackoverflow thread: https://stackoverflow.com/questions/75043889/manifest-v3-background-scripts-service-worker-on-firefox

This may change in the future but for now I wanted to get a firefox version working so that I could use my dumb little tool personally as I don't really use chrome.

I also decided along the way that I would just port it over to the browser object namespace since I was already tinkering around, and remove a few superfluous things/try to optimize if I could. 

## Things noticed along the way
This actually went really smoothly, there were no issues whatsoever with the browser apis. I probably didn't need to even convert from Chrome to Browser namespace but whatever. The things that **DID** need to change were:
1. The manifest entry for the background script as mentioned above
2. The "commands" permission that was present in the manifest for the chrome version was not neccessary in this version and threw a warning in firefox, so I removed it
3. In order to see logs while developing, you cannot use the regular browser console like you can in chrome. This threw me off for a while and was the only difficulty in making this port really. To see logs while developing the extension you have to go to `about:debugging`, then select 'this firefox' and finally click the inspect button on your extension. This will bring up a special development console where you will see the background scripts logs.