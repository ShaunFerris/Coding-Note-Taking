#projects 

## Outline
A simple static site project to display a private collection of artworks inhereted by a family member, that may be of public interest as they have not been exhibited publicly before. 

Three main points will make up the UI and user flow:
1. An attractive splash page that communicates the mission statement of the site - to display previously unknown works of the artist in question![[v1homepage.png]]
2. Gallery - scans of the works in a grid layout. Clicking an image enlarges it and give a carousel to explore larger versions of all images, alongside a short description (maybe)
3. Contact form/about - Introduce the holder of the collection and invite the users to make contact if they would like to arrange a showing of the art works or learn more.

### Additional features/pages requested
The above outlines the initial design based on my first conversation with the client, and my understanding of the requirements based on that conversation. Upon further discussion, they suggested other desireable features. These will likely require the site to be a multipage app, so thankfully I had already started the project with next.js and routing between multiple pages will be easy enough. The requested features/routes are:
1. A section for currently on sale works with pictures and links to the associated auction house. This will likely be on a separate page from the main collection gallery to highlight that they are seperate.
2. Link to upcoming exhibitions of the artists work. Could possibly implement this in a blogpost style fashion.
3. Past exhibitions page. Not sure how this should be implemented, will need to discuss more.
4. A page detailing who the artist studied with and when. Could this be blended into a comprehensive bio page for the artist?

## Technical requirements
<span style="color: cyan; font-weight: bold;">The images need to load fast, reliably and be supported with accessibiltiy features.</span> Will likely want to load every image in multiple sizes so the the transition between gallery thumbnail/carousel image/full size image with blurb is seemless.

Using cloudinary as a CDN to host the images and handle transformations. This is likely the only real backend functionality that the site will need. It's possible this will change in the future if the client wants to go further and implement mail lists and registration but for now image delivery and transformations is it.

Framer motion will be used for animation of image transitions in the carousel component, as well as general UX animations and transitions. Framer motion is crictical to this project for the handling of image presentation, but this also presents me with and opportunity to practice using it and to practice making a site look really polished and modern. I really didn't notice how many modern sites have animated UI until I started thinking about image carousels.

## Design notes
**TODO**

## Component design/heirarchy
NOTE: diagrams in this section are NOT layout diagrams meant to represent the way the site looks. They are components diagrams mocking up plans for how the react components will be connected to each other in the vDOM tree structure.

### Gallery route
Here is a diagram for the current idea of how the image gallery will be designed. ![[Pasted image 20230829184945.png]]The architecture is done this way so that image fetching is done client side for speed and security, and because the more we can do on the client side the better the SEO will be, and this is the first project I have done where SEO actually matters a bit.

Actually ended up implementing this a little differently, I didn't use a component for the modal but instead used one client component that has the code for both the gallery grid and the modal, and uses framer motion to overlay the modal full-screen over the grid on click, then get rid of it again on a second click. I will eventually add a carousel to this so that you can click through the images, and at that point I may need to go back to the architecture in this diagram, but for now I am enjoying trying to lower the amount of code splitting and components that I usually tend to make.

### Animated page transitions with a wrapper component
In service of making the site smooth and modern, I wanted pages to animate in and out with a fade effect when the user navigates. This is fairly simple to do with framer motion by having an outer element on every page by a motion variant and then wrapping all pages in an `<AnimatePresence/>` element. However, all the framer motion logic is client side, which would make it necessary for EVERY page route in the app to be client side.

Because the main page file for the gallery route was written as a React Server Component to take advantage of server side async fetching of the images, using framer motion functions or components directly inside it was impossible, which presented some problems as I wanted transitions between all the routes to animated for UX and for my own practice in making modern feeling sites.  This was eventually solved by writing a `<PageWrapper />` component that can wrap all the content of any route from within a client side component, and then that client side component can be a child of the RSC. This wrapper is reused on all the routes to provide the animation capability, but the gallery was where it's implementation was most tricky. The Component heriarchy for the gallery route at this point looks like this: 

**SKETCH GOES HERE**

The downside of doing the page transition animation like this is that I cannot use animate presence to animate the exit of the page when a route change occurs. This is because with the wrapper component set up the way it is, the wrapper un-mounts when the path changes, so the animate presence cannot use the path change as it's condition. To get it working, the `<AnimatePresence/>` element would have to be far enough up the DOM tree, like in the layout for example, that it was still mounted when the route changed (I'm pretty sure). I have not been able to think of a good way to have the `<AnimatePresence/>` element wrap the page transition logic in such a way that it doesn't un-mount on navigation without running up against the same issue from before with the RSC nature of the gallery component.

## Dev diaries
For granular notes on my day to day process of working on getting this site up and running see these notes: 
- [[Project Notes - Kerns Collection Gallery - 01 - 02.09.2023 - Current Status]]