#projects 

This was a small little side project that I put together on a whim, and serves as a good parable about my experiences with both the good and bad of using generative AI like GPT in your work flow.

## What is spongecase and what is the project?
SpoNGeCAse IS thE cAsiNG forMAT FrOm tHe MoCKinG SpoNGEBob MeMe. The idea for this project was a simple chrome extension that would run in the background listening for a hotkey. When it registered a press it would run a simple string conversion on any highlighted text and then copy the converted string to the clipboard where it could be pasted wherever the user wanted. The general usecase is mocking people online by easily taking what they said, converting it to this case and saying it back to them.

## Process
#### TLDR: use gpt to get an idea of a projects scope, then STOP using it
I used chat-gpt to get an idea of how hard this project might be. This is the part that went well, it gave me good simple instructions for which parts of the chrome APIs I was likely to need to use, what aspects of project structure I should pay attentions to, and simple instructions for testing my extension. I was able to scaffold the project and get a working extension up and running in probably less than an hour, including writing the spongecase algorithm without help for algorithm practice.

At this point I wanted to add a notification system to the extension, and this is where I fucked up by involving GPT in the process too much. I used code that it wrote and it didn't work. I asked again a few times, correcting it and trying different prompt set ups and it never worked and it's code was shit. **I spent way too much time doing this and it was a huge mistake**.

I then went on without GPT to figure it out, but I still had bits of code it had written and this was another huge mistake, it had written background code with promise chains which I don't have alot of practice with, and which make it hard to debug. This messy chained code meant that I went on banging my head against a wall and wasted a full day. It was only when I calmed down and re-wrote from the ground up with async/await syntax that I found the bug I was struggling with.

## The extension itself
The actual code is very simple. The file structure of a chrome extension is: ![[Pasted image 20230907104504.png]]
In this project I only needed to have a service worker for the background code, as well as a manifest and an icon because they are mandatory. See [here](https://developer.chrome.com/docs/extensions/mv3/architecture-overview/) for more info on extension architecture.

The project uses manifest V3 to tell the browser what permissions it needs, and where the code is. you can read about it [here.](https://developer.chrome.com/docs/extensions/mv3/intro/)

All the logic is in a backround.js file and it uses the [chrome commands API](https://developer.chrome.com/docs/extensions/reference/commands/)to add an event listener for the hotkey command, the [chrome scripting API](https://developer.chrome.com/docs/extensions/reference/scripting/) to run the function that converts and copies text, and the [chrome notifications API](https://developer.chrome.com/docs/extensions/reference/notifications/) to send notifications based on the result. All of these APIs have equivalents for firefox and other browsers too.

## Firefox port
I also ported this extension to work with firefox too, details on that are here: [[Project Notes - Porting the Spongecase Extension to Firefox]]