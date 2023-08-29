#projects 

## Outline
A simple static site project to display a private collection of artworks inhereted by a family member, that may be of public interest as they have not been exhibited publicly before. 

Three main points will make up the UI and user flow:
1. An attractive splash page that communicates the mission statement of the site - to display previously unknown works of the artist in question![[v1homepage.png]]
2. Gallery - scans of the works in a grid layout. Clicking an image enlarges it and give a carousel to explore larger versions of all images, alongside a short description (maybe)
3. Contact form/about - Introduce the holder of the collection and invite the users to make contact if they would like to arrange a showing of the art works or learn more.

## Technical requirements
The images need to load fast, reliably and be supported with accessibiltiy features. Will likely want to load every image in multiple sizes so the the transition between gallery thumbnail/carousel image/full size image with blurb is seemless.

Using cloudinary as a cdn to host the images and handle transformations. This is likely the only real backend functionality that the site is likely to need.