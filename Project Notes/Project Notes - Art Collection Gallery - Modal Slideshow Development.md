#projects 

## Work completed so far
As of 12.09.2023 a working version of the art gallery route is completed. When on the gallery route a gird of images fetched from the cloudinary back end are displayed. When one of theses images is clicked the full screen slideshow displays with an animation, with a larger image and the description taking up about half the space each. There is a button top right to exit the slideshow back to the gallery grid, and a button at each end that causes an animated transition to the next image in the gallery set. 

## Priorities for continued development
1. Look into utilising suspense on the gallery route, taking advantage of the lengths gone to so far to utilise RSCs
2. Consider converting from using state to store the current image ID to using query params. This will allow linking directly to a given place in the slideshow
3. Write tests for the gallery route
4. Consider adding a hover animation to the images in the gallery grid, perhaps showing a slightly zoomed in version
5. Add keyboard navigation to the modal
6. Look into tidying up the animations further, currently the exit animation when the modal is closed looks really wonky
