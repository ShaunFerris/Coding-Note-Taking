#webdevSpecific #frontend #buildtools

**The official Vite docs are [here](https://vitejs.dev/guide/)**

Vite is a build tool that is specifically built to be fast and lean for modern web projects. It is the build tool that I will be using for creating static web pages with react.

At it's core, vite does two things:
1. Serve your code locally during development
2. Bundle your JS, CSS, and other assets together for production, in a way that confers performance benefits.

There are may tools out there, like webpack, that do the same thing, but Vite is both simplified and faster. Prior to ES6 where module imports were introduced, there was no native way to bundle js together in modular fashion. This is where tools like webpack came in, to concatenate multiple files together into a single bundle for the browser. This process becomes increasingly slow as the app adds more code and dependancies. Now that modules have been introduced in ES6 and browsers widely support this feature, Vite uses modular importing to load code instantly no matter how large the app is. This allows it to also support hot swapping of modules during development which is a fantastic quality of life feature.

## Scaffolding a project with vite
Getting started with Vite is very simple. With Node.js installed, simply run:
```bash
npm create vite@latest
```
If create-vite is not yet installed you will be prompted to approve it. Then you simply follow a set of prompts in the CLI to decide what kind of project to make, be it vanilla, react, vue etc, and don't need to worry about installing any of that tooling.

The last step of the process is to run npm install to set up the dependencies, then npm run dev to start a dev server.

