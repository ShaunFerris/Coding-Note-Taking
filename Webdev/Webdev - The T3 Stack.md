#webdevSpecific 

T3 is a webdev stack that focusses on simplicity, modularity and typesaftey end to end. The docs are [here](https://create.t3.gg/en/introduction).

Going forward from today (20.08.2023) I am going to be picking this stack up. It builds on tech I already use like Next + React + TS, but introduces Trpc and Prisma connected to planetscale. The technologies put together in the stack offer a lot of great benefits to dev experience and the build quality of the final product, and work really well with service providers like Planetscale and Vercel that have extrememly generous free tiers with great dev ex.

<span style="color: cyan; font-weight: bold;">HUGE NOTE: The utility create-t3-app still uses the pages router in Next.js, and I prefer/only know the app router. I will still be using the elements of the t3 stack, but have to set up and integrate manually .</span>

## Technologies that make up the stack
Below are notes with quick summaries of the individual technologies that are a part of the stack.
- [[Webdev - Next.js 13 - Basic Introduction]] Next is the main frameword around react, handling routing, webpacking and chunking of code, SSR etc
- [[Typescript - Introduction and Hub]] Built in preconfigured TS support for static testing with typechecks
- [[Webdev - TRPC]] Remote procedure call library using the TS server to check typesafety of communications between the back and front end
- [[Webdev - Tanstack Query]] A library that extends the fetch api to give you powerful tools for sending queries to back end resources, and handles global state management, data mutations and subscriptions
- [[Webdev - Prisma ORM]] Prisma handles the object relational mapping of data models to SQL databases

## Getting started - Create T3 app
You can scaffold an app with the stack using:
`npm create t3-app@latest`
This will run a cli tool that runs you through the options of what to include, you are not expected to use every part of the stack everytime, and are also encouraged/expected to add other libraries and tools depending on your application.

## T3 Axioms - From their own docs site [here](https://create.t3.gg/en/introduction)
We’ll be frank - this is an _opinionated project_. We share a handful of core beliefs around building and we treat them as the basis for our decisions.

### Solve Problems
It’s easy to fall into the trap of “adding everything” - we explicitly don’t want to do that. Everything added to `create-t3-app` should solve a specific problem that exists within the core technologies included. This means we won’t add things like state libraries (`zustand`, `redux`) but we will add things like NextAuth.js and integrate Prisma and tRPC for you.

### Bleed Responsibly
We love our bleeding edge tech. The amount of speed and, honestly, fun that comes out of new shit is really cool. We think it’s important to bleed responsibly, using riskier tech in the less risky parts. This means we wouldn’t ⛔️ bet on risky new database tech (SQL is great!). But we happily ✅ bet on tRPC since it’s just functions that are trivial to move off.

### Typesafety Isn’t Optional
The stated goal of Create T3 App is to provide the quickest way to start a new full-stack, **typesafe** web application. We take typesafety seriously in these parts as it improves our productivity and helps us ship fewer bugs. Any decision that compromises the typesafe nature of Create T3 App is a decision that should be made in a different project.
