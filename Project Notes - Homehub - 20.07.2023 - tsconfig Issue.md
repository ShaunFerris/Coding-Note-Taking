#projects 

As part of the push to implement major new features and progress homehub towards a version 1.1 release, I have made the decision to implement all new features in typescript, and eventually adopt it across all code in the app. 

With that in mind today wanted to get the app ready to use typescript, and in the process discovered some very strange behaviour that broke the build and caused about 3 hours of rabbit hole diving to find a fix. **I did not actually end up finding any fix online, and arrived at the fix on my own eventually, this issue is not well documented**

## How the issue arose
After writing my first typescript yesterday, implementing a new mongoose schema for the upcoming usergroups feature, today I realised that I was not actually compiling typescript properly. Looking at the next.js docs I saw that to implement TS in an existing project, all you need to do is create a blank tsconfig.json file, and then run the dev server or build and next will automatically populate the config for you. I did this with no trouble and pushed.

Then I got an email from vercel saying that the build had failed.

The build had failed due to multiple (maybe even all?) imports of local files failing. For example importing a component into a page file, or importing the globals.css file into the main layout file. The message from next was a module not found error, which made no sense because the modules were all right there in the project source where they had always been.

After some tinkering and reading a lot of forum posts, I eventually figured out that deleting the tsconfig magically fixed everything again. At this point I was thoroughly confused. Deleting or commenting out lines in the ts config did not fix anything.

## The fix
Eventually I stumbled across an article about adoption of TS in large JS projects, pointing out that it is not possible to have both a jsconfig and a tsconfig in the same project, the compiler will always pick one. So I deleted my existing jsconfig, and the issue persisted. Then I deleted the tsconfig without restoring the js config, and the issue still persisted. This highlighted that there must be something in the jsconfig that I needed, as with the  js but not the ts config it worked, with both it did not, presumably because the compiler was preferring the ts one.

I went back through git history and got the contents of the jsconfig, which was generated for me on project setup:
```json
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```
And suddenly everything made sense. Adding a tsconfig, building so that next populated it for me, and then adding the above to the compiler options fixed the issue. 

It appears that when you start a project with ts, this option is added to the tsconfig, but not if you convert to ts midway through a project initialized as a js one.

I ended up submitting a pull request to add a sentence to the docs, in the section that talks about using typescript in an existing next project, instructing users to copy over the paths compiler option. Hopefully it gets accepted as that would have saved me three hours or so.