#frontend #frontend-libs #webdevSpecific 

## What is BootStrap?
Bootstrap is a free and open source front-end framework for building resposive web-pages and applications. In this context, responsive simply means that the page or app is capable of adapting to many different device sizes and viewports.

Part of designing a website is designing how different frames and menus are positioned on the page and in relation to each other, and this is controlled by css. However, writing css that can adapt to many different viewports, or writing it again whenever you want to reuse a layout is a chore, and bootstrap aims to help here.
<blockquote style="font-weight: bold; color: cyan;">
As a framework, Bootstrap is a collection of pre-written code chunks in CSS, HTML, and JavaScript that allows developers to create websites more quickly than if they had to create every bit of code from scratch. Bootstrap saves developers time so that they can focus on other things rather than coding CSS.</blockquote>

For a comprehensive overview see the bootstrap docs [here](https://getbootstrap.com/docs/5.0/getting-started/introduction/).

## Using Bootstrap
You can add bootstrap to an existing or new project in two ways:
1. If you are using a package manager or need to download the source files, head to [getbootstrap.com](https://getbootstrap.com/docs/5.0/getting-started/download/) to download them. There are instructions at that web address to follow.
2. The most convenient way: use jsDelivr, a free and open source CDN. This is the method that will be covered here in more detail.

### Bootstrap CSS
Copy-paste the stylesheet `<link>` into your `<head>` before all other stylesheets to load BS CSS.
```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
```

### Bootstrap JavaScript
Many BS components require the use of JavaScript to function. Specifically, they require Bootstraps own JavaScript plugins and [Popper](https://popper.js.org/). Place **the following bundle `<script>`** near the end of your pages, right before the closing `</body>` tag, to enable them. If you don't want to use the whole bundle, but rather load them in seperately or as modules, see the docs on [this site](https://getbootstrap.com/docs/5.0/getting-started/introduction/) for more info.

#### Bundle
Include every Bootstrap JavaScript plugin and dependency with one of our two bundles. Both `bootstrap.bundle.js` and `bootstrap.bundle.min.js` include [Popper](https://popper.js.org/) for our tooltips and popovers. For more information about what’s included in Bootstrap, please see our [contents](https://getbootstrap.com/docs/5.0/getting-started/contents/#precompiled-bootstrap) section.
```html
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous"></script>
```

## Basic usage
A few very basic use-cases of bootstrap will be outlined here to give the general idea of how bootstraps css and js are invoked. More detailed notes may be added in the future, otherwise look at the docs.

### Containers
Once you have included bootstrap in your project, you can get started with basic implementation. A good first step is to use one of bootstraps containers. 

Containers are the most basic layout element in Bootstrap and are **required when using the default grid system**. Containers are used to contain, pad, and (sometimes) center the content within them. While containers _can_ be nested, most layouts do not require a nested container.

Bootstrap comes with three different containers:
-   `.container`, which sets a `max-width` at each responsive breakpoint
-   `.container-fluid`, which is `width: 100%` at all breakpoints
-   `.container-{breakpoint}`, which is `width: 100%` until the specified breakpoint

Invoke these containers by adding the desired container-name as the class of a div. Wrapping all the content of your page in a simple fluid container div will keep the content at 100% width at any screen size. Using a .container div would keep the max width the same until a viewport size breakpoint is hit, then change it.

### Responsive images
Another simple use is to make images scale with parent element. This can be achieved by simply adding the `.img-responsive` class to the `<img>` element.

### Responsive text centering
You can make sure that heading text stays centered at any viewport size by giving the element the `text-center` class.

### Use Bootstrap button styling
Bootstrap has its own styles for `button` elements, which look much better than the plain HTML ones. Include these in your work by giving a button element the `btn` and `btn-default` classes.

Normally, your `button` elements with the `btn` and `btn-default` classes are only as wide as the text that they contain. For example:
```html
<button class="btn btn-default">Submit</button>
```
This button would only be as wide as the word `Submit`.

By making them block elements with the additional class of `btn-block`, your button will stretch to fill your page's entire horizontal space and any elements following it will flow onto a "new line" below the block.
```html
<button class="btn btn-default btn-block">Submit</button>
```
This button would take up 100% of the available width.
`btn-danger`, `btn-primary` and `btn-info` are options to replace `btn-default` that give you other `btn-info`color options.

## More bootstrap
For more notes on bootstrap see these entries:![[Pasted image 20230506130159.png]]
[[Web Front-End - Bootstrap - The Grid System]]