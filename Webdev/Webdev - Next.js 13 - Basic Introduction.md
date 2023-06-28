#webdevSpecific #frontend #fullstack 

NOTE: These notes are specific to Next 13+ and will cover only the use of the App router, not the older pages router. While the page router still works in Next 13, it is primarily for backwards compatibility and to allow incremental adoption of the new app router.

These notes are heavily based on the offical docs as of June 2023, and are essentially that content but edited down for brevity/refocussed on things I personally wanted to go over. As such the notes will often link to the docs.

## What is Next?
Next is the most popular full stack javascript framework, that uses react for building the front end, but wraps this with the ability to route between pages, write APIs and interact with databases.

React is relatively unopinionated about how you build and structure your applications - there are multiple ways to build applications with React. Next.js provides a framework to structure your application, and optimizations that help make both the development process and final application faster.

Next provides APIs and options to manage:
- The dev and productions environments, with relevant packaging and minification optimizations
- Buildtime and Runtime options for running your code, so you can run static code at buildtime to make your app more light weight.
- Client and server side rendering of your code, again providing a sliding scale between optimization and dynamic interactivity

## Dev and prod environments
Next.js provides features for both the development and production stages of an application. For example:

- In the development stage, Next.js optimizes for the developer and their experience building the application. It comes with features that aim to improve the **Developer Experience** such the [TypeScript](https://nextjs.org/docs/basic-features/typescript) and [ESLint integration](https://nextjs.org/docs/basic-features/eslint), [Fast Refresh](https://nextjs.org/docs/basic-features/fast-refresh), and more.
- In the production stage, Next.js optimizes for the end-users, and their experience using the application. It aims to transform the code to make it performant and accessible.

Since each environment has different considerations and goals, there is a lot that needs to be done to move an application from development to production. For instance, the application code needs to be [compiled](https://nextjs.org/learn/foundations/how-nextjs-works/compiling), [bundled](https://nextjs.org/learn/foundations/how-nextjs-works/bundling), [minified](https://nextjs.org/learn/foundations/how-nextjs-works/minifying), and [code split](https://nextjs.org/learn/foundations/how-nextjs-works/code-splitting).

## Compiling
Developers write code in languages that are more _developer-friendly_ such as JSX, TypeScript, and modern versions of JavaScript. While these languages improve the efficiency and confidence of developers, they need to be compiled into JavaScript before browsers can understand them.

Compiling refers to the process of taking code in one language and outputting it in another language or another version of that language.
![](https://nextjs.org/static/images/learn/foundations/compiling.png)
In Next.js, compilation happens during the development stage as you edit your code, and as part of the build step to prepare your application for production. Next compiles your jsx and es7+ js into es5 js that the browser can run easily and with good performance.

## Minifying
Minification is the process of removing unnecessary code formatting and comments without changing the code’s functionality. The goal is to improve the application’s performance by decreasing file sizes.

In Next.js, JavaScript and CSS files are automatically minified for production.
![](https://nextjs.org/static/images/learn/foundations/minifying.png)

## Bundling and Code Splitting
Devs write modern apps using many files, for example in react we break things up into lots of components. This makes it easier to follow and to keep things from becoming too tightly coupled.

Bundling is the process of resolving the web of dependencies and merging (or ‘packaging’) the files (or modules) into optimized bundles for the browser, with the goal of reducing the number of requests for files when a user visits a web page.

Developers usually split their applications into multiple pages that can be accessed from different URLs. Each of these pages becomes a unique **entry point** into the application.

Code-splitting is the process of splitting the application’s bundle into smaller chunks required by each entry point. The goal is to improve the application's initial load time by only loading the code required to run that page.

## Buildtime vs runtime
**Build time** (or build step) is the name given to a series of steps that prepare your application code for production.

When you build your application, Next.js will transform your code into production-optimized files ready to be deployed to [servers](https://nextjs.org/learn/foundations/how-nextjs-works/client-and-server) and consumed by users. These files include:

- HTML files for statically generated pages
- JavaScript code for [rendering](https://nextjs.org/learn/foundations/how-nextjs-works/rendering) pages on the [server](https://nextjs.org/learn/foundations/how-nextjs-works/client-and-server)
- JavaScript code for making pages interactive on the [client](https://nextjs.org/learn/foundations/how-nextjs-works/client-and-server)
- CSS files

**Runtime** (or request time) refers to the period of time when your application runs in response to a user’s request, _after_ your application has been built and deployed.

## Further Reading on Next.js
These have been the very basics of what next is, to continue, see topics below:

- [[Webdev - Next.js 13 - Client and Server Rendering]]
- [[Webdev - Next.js 13 - Routing]]
