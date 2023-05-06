#frontend #frontend-libs #javascript #webdevSpecific 

Bootstrap uses a responsive 12-column grid system, which makes it easy to put elements into rows and specify each element's relative width. Most of Bootstrap's classes can be applied to a `div` element.

Bootstrap has different column width attributes that it uses depending on how wide the user's screen is. For example, phones have narrow screens, and laptops have wider screens.

Take for example Bootstrap's `col-md-*` class. Here, `md` means medium, and `*` is a number specifying how many columns wide the element should be. In this case, the column width of an element on a medium-sized screen, such as a laptop, is being specified.

For example, say you have three buttons on a webpage:
```html
<button class="btn btn-block btn-primary">Like</button>
<button class="btn btn-block btn-info">Info</button>
<button class="btn btn-block
```
Written like this, the buttons will display like this:
![[Pasted image 20230506124546.png]]

But if you wrap all three buttons in a div with the row class, and then each button individually in a div with a col class, you can display them side by side:
```html
<div class="row">
	<div class="col-xs-4">
		<button class="btn btn-block btn-primary">Like</button>
	</div>
	<div class="col-xs-4">
		<button class="btn btn-block btn-info">Info</button>
	</div>
	<div class="col-xs-4">
		<button class="btn btn-block btn-danger">Delete</button>
	</div>
</div>
```
![[Pasted image 20230506125002.png]]
