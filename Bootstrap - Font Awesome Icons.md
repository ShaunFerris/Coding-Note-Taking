#frontend #frontend-libs #webdevSpecific 

Font Awesome is a library of icons that can be either webfonts or vector graphics. Font Awesome icons are treated just like fonts, you can specify their sizes in pixels, and they will assume the font-sizes of their html parent elements.

Font awesome is often used with bootstrap but is a seperate library that needs to be included seprately with it's own CDN link:
```html
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.8.1/css/all.css"integrity="sha38450oBUHEmvpQ+1lW4y57PTFmhCaXp0ML5d60M1M7uH2+nqUivzIebhndOJK28anvf"
crossorigin="anonymous">
```

The `<i>` element in html was originally used to make text enclosed in it italic, but was no longer needed when CSS became common, and the CSS font-style property could be used for this purpose. Now, the `<i>` element is instead commonly used for icons.

Adding Font Awesome classes to an `<i>` tag will display the element as an icon:
```html
<i class="fas fa-info-circle"></i>
```
The above code will display this icon:
![[Pasted image 20230506132225.png]]
Note: it is also totally fine to use span elements for this purpose instead of i elements.