#testing #javascript 

The first time you mount _any_ new component, you may notice that the component doesn't look like it should. Unless your application is written _exclusively_ using Component-scoped CSS (e.g. Styled Components or Vue's Scoped Styles) you will need to follow this guide in order to get your component looking **and behaving** like it will in production.

We build our applications within the context that they're supposed to run in, and we make assumptions that our components will always be rendered within a root-level component (such as an `<App>`) or a top-level selector with style rules (such as `#app { /* styles in here */ }` )

When we attempt to isolate our component to put it under test, we need to put that environment back together. We'll go into that in a moment. First, let's talk about stylesheets, testing, and one of Cypress's biggest differences in contrast to other component testing tools.

## Rationale for testing components with styling
