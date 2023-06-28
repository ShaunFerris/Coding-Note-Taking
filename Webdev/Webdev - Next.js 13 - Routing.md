#webdevSpecific #frontend #fullstack 

Next uses file system routing, where the names of directories define the actual URL structure of the webapp. This means that each directory that is a sub-folder of the app directory represents a page of the website or app, with the app directory itself containing a page.js and layout.js file that define the structure and content of the homepage. 

When a directory is surrounded by square brackets, that means that it is a dynamic route, which might be like an id or username that can be any wildcard vaule. 

To clarify, routes in webdev are URL schema. For example:
```
www.mysite.com/blog/{dailypost}
#The above URL routes from home to the blog page to the blog of the day page., with the blog of the day being dynamic as it is generated every day.
```

Directories can also be surrounded by parentheses, which means they will be ignored by the routing system of Next, allowing you to organise files that work on the homepage into their own folders if you want to, or if you simply want to put some files somewhere without them affecting the actual url structure of your app.

## Page and layout
Next has some files names that are reserved for interacting with the file system based app router. You can't just name any old file with reserved names as they will be treated in special ways by the routing system. The most common of these that you will interact with are `page.js` and `layout.js`. 

Layout is a react component that takes children and renders them inside the body of an html element. Any content defined here will be rendered on any page in the app. Layouts can be nested, so a layout in a sub-folder of the app directory will only apply to the page it represents and children of that page.

Page is a react component that defines the content to render for the current page route. ie: the current sub-directory of the app folder, with app being the homepage or topmost level page of the site.
