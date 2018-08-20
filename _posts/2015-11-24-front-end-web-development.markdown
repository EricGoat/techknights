---
layout: post
title: Front End Web Development
date: 2015-11-24 00:00:00 -0400
author: Eric Colon
---

Welcome to the Knight Hacks Front End Web Development workshop! This is part 1 in a series of workshops in which we will be creating a functional to-do list app.

Begin by cloning down this repo containing the necessary files for our app:
`git clone https://github.com/KnightHacks/frontend-workshop.git`

Navigate to the directory you cloned the repo down to and Open `index.html`. As you can see there's not much in it:

```html
<!doctype html>
<html>
	<head>
		<title>Todo List</title>

		<link href="https://fonts.googleapis.com/css?family=Roboto+Slab:400,700|Roboto+Condensed:400,700" rel="stylesheet" type="text/css">
		<link rel="stylesheet" href="todolist.css">
	</head>

	<body>

		<script src="todolist.js"></script>
	</body>
</html>
```

This is just some basic scaffolding that links to an external Google Fonts stylesheet, and the `.css` and `.js` files we will be working on later.

Let's begin scaffolding the actual to-do list. Add this block of HTML after the opening `<body>` tag:

```html
<main class="TodoListContainer">
  <div class="TodoList">
    <h1 class="title">To-do</h1>

    <div id="itemsList" class="itemsList">
    </div>

    <div class="magic">
      <button class="addButton" id="addButton">+</button>
    </div>
  </div>

  <div class="inputBoxContainer">
    <div class="inputBox addItem">
      <input type="text" id="newTodoLabel" placeholder="To-do item...">
    </div>
  </div>
</main>
```

We've added quite a few elements, so let's break this down a bit.

`<main class="TodoListContainer">` is acting as a container for the entire to-do list, including the input field for us to add new items. It has a `class` attribute set to `ToDoListContainer`.

`<div class="TodoList">` is the actual to-do list card. It contains an `<h1 class="title">`, a `<div id="itemsList" class="itemsList">`, and a `<div class="magic">` with a `<button class="addButton" id="addButton">` as its child. That element is the button that will reveal an input field for us to add new items with.

`<div class="inputBoxContainer">` is a container for our input field and `<input type="text" id="newTodoLabel" placeholder="To-do item...">` is the input field. Notice it has a `placeholder` attribute set to `To-do item...`, this string acts as placeholder text for the input field until we begin writing content in it.

Now, add this remaining bit of HTML after the previous block we've added.

```html
<div id="itemTemplate" class="template item">
  <div class="checkBox">
    <input type="checkbox" id="item_id">
  </div>

  <div class="itemTitle">item_title</div>
</div>
```

This bit of HTML is serves as a template for new items in our to-do list. It will be hidden by default using CSS, then, when a new item is added to the list, it will be copied and added as a child into the `<div id="itemsList" class="itemsList">`.

Your file should now look like so:

```html
<!doctype html>
<html>
	<head>
		<title>Todo List</title>

		<link href="https://fonts.googleapis.com/css?family=Roboto+Slab:400,700|Roboto+Condensed:400,700" rel="stylesheet" type="text/css">
		<link rel="stylesheet" href="todolist.css">
	</head>

	<body>
		<main class="TodoListContainer">
			<div class="TodoList">
				<h1 class="title">To-do</h1>

				<div id="itemsList" class="itemsList">
				</div>

				<div class="magic">
					<button class="addButton" id="addButton">+</button>
				</div>
			</div>

			<div class="inputBoxContainer">
				<div class="inputBox addItem">
					<input type="text" id="newTodoLabel" placeholder="To-do item...">
				</div>
			</div>
		</main>

		<div id="itemTemplate" class="template item">
			<div class="checkBox">
				<input type="checkbox" id="{{item_id}}">
			</div>

			<div class="itemTitle">{{item_title}}</div>
		</div>

		<script src="todolist.js"></script>
	</body>
</html>
```

That's it as far as the HTML is concerned; let's style everything now. Open the `todolist.css` file.

```css
* {
	box-sizing: border-box;
}

body {
	padding-top: 20px;

	font-family: Roboto, sans-serif;

	background: #efefef;
}

.template {
	display: none;
}
```

Some base styling has been included. We're using the `*` selector to target all elements and assign them a `box-sizing` property set to `border-box`. The `border-box` value makes the `padding` and `border` of an element fill the inside of the element, instead of outside by default. It's a useful trick that helps with managing layouts.

Add this CSS after `.template`:

```scss
.TodoListContainer {
	position: relative;

	margin: auto;
	width: 350px;
}

.TodoList {
	position: relative;

	padding-bottom: 40px;

	background: #fafafa;
  box-shadow: 0 2px 2px 0 rgba(0,0,0,.14),
							0 3px 1px -2px rgba(0,0,0,.2),
							0 1px 5px 0 rgba(0,0,0,.12);
}

.TodoList .title {
	padding: 20px;

	font-family: 'Roboto Condensed', sans-serif;
	font-size: 20px;
	font-weight: 700;

	border-bottom: 1px solid rgba(0,0,0,0.1);
}

.itemsList {
	position: relative;
	display: block;
}

.addButton {
	display: block;
	height: 56px;
	margin: auto;
	padding: 12px 0;
	overflow: hidden;
	width: 56px;

	font-size: 24px;
	line-height: normal;
	text-align: center;

	background: #f44336;
	border: 0;
	border-radius: 50%;
	box-shadow: 0 2px 2px 0 rgba(0,0,0,.14),
							0 3px 1px -2px rgba(0,0,0,.2),
							0 1px 5px 0 rgba(0,0,0,.12);
	color: #fafafa;

	transition: background 0.25s ease;
}

.addButton:hover {
	background: #ff5e52;
}

.addButton:active {
	background: #c12b20;
}

.magic {
	position: absolute;
	right: 0;
	bottom: -28px;
	left: 0;

	width: 100%;
}
```

As you can see, that's a lot of CSS, so it won't be very time efficient to do anything other than a high-level explanation of all this.

`.TodoListContainer` has been assigned a `position`, `margin`, and `width` property. Thanks to the `margin` property being set to `auto`, this has given us a centered container for our content.

`.TodoList` has been styled into an off-white card with a subtle shadow and bottom padding for the "add" button.

`.TodoList .title` has been styled to have padding around it, its typeface set to a bolded form of "Robot Condensed", and a faint bottom border.

`.addButton` has a lot of styling applied to it that's necessary to give it a nice "Material Design" look and feel.

`.addButton:hover` changes the background of the button to a lighter shade of red when hovered over.

`.addButton:active` changes the background of the button to a darker shade of red when clicked.

`.magic` is a special container for the button. It's the full width of the to-do list card and assists with centering the button and moving it halfway off the edge of the to-do list card.

We're almost there! Add this remaining CSS after `.magic`:

```scss
.item {
	padding: 10px 20px;

	font-family: 'Roboto Slab','Times New Roman',serif;
	word-wrap: break-word;

	border-bottom: 1px solid rgba(0,0,0,0.1);
	color: rgba(0,0,0,0.8);
	outline: none;
}

.item .itemTitle {
	display: inline-block;

	font-weight: 600;
}

.item .checkBox {
	display: inline-block;
}

.item.checked .itemTitle {
	text-decoration: line-through;

	color: rgba(0,0,0,0.4);
}

.inputBox {
	padding: 20px;
	width: 100%;

	background: #fafafa;
}

.inputBoxContainer {
	height: 0;
	margin-top: 38px;
	overflow: hidden;

	box-shadow: 0 2px 2px 0 rgba(0,0,0,.14),
							0 3px 1px -2px rgba(0,0,0,.2),
							0 1px 5px 0 rgba(0,0,0,.12);

	transition: height 0.25s ease;
}

.inputBoxContainer.visible {
	height: 80px;
}

.inputBox input {
	background: rgba(0,0,0,0.1);
	border: none;
	border-bottom: 1px solid rgba(0,0,0,.12);
	display: block;
	font-size: 16px;
	margin: 0;
	padding: 10px;
	width: 100%;
	text-align: left;
	color: inherit;
}
```

Once again, a high-level explanation will be necessary for all of this.

`.item` has been given a nice amount of padding, the "Roboto Slab" typeface, a bottom border, and a faded black text color.

`.item .itemTitle` has been assigned a `position`, `padding-bottom`, `background`, and `box-shadow` property. The result is an off-white card with a subtle shadow that bottom padding for the "add" button.

`.item .checkBox` has been styled to have padding around it, its typeface set to a bolded form of "Robot Condensed", and a faint bottom border.

`.item.checked .itemTitle` adds a line through the next of a to-do list item and fades out its color.

`.inputBox` mimics the white background and padding of the main to-do list card.

`.inputBoxContainer` hides the input field by having its `height` set to 0 and its `overflow` property set to `hidden`. This will hide any content that overflows from this element, and since it has a height of 0 that means everything within it will be hidden. Then a shadow is added and a [special transition property](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions). The `transition` property controls animation speed and applies timing functions when a property changes. In this case, an easing function will be applied to the height of this element over a span of `0.25` seconds whenever its changed.

`.inputBoxContainer.visible` this sets the height of the container to 80px.

`.inputBox input` these styles give the input field a faded black background, bottom border, padding, and makes it span the width of the container.

Phew, we're done with the CSS for this thing! If you open `index.html` in your browser, you should have a gorgeous Material Design inspired to-do list!

Let's make this work now. Open `todolist.js`:

```javascript
document.addEventListener('DOMContentLoaded', function() {
	var items    = [];
	var inputBox = document.querySelector(".inputBoxContainer");

  // the magic happens here
});
```

Some groundwork has been laid out for us already. As explained in the [intro workshop](/workshops/intro-web-development), we're placing all of our code within an anonymous function that will be called when the DOM finishes loading. This is to prevent the infamous FOUC (flash of unstyled content). We've also initialized two variables, `items` and `inputBox`. The `items` variable has been assigned as an empty array for now, while `inputBox` has been assigned the `<div>` with its class set to `.inputBoxContainer`.

Let's get the button working now. Replace `// the magic happens here` with this code:

```javascript
document.querySelector("#addButton").addEventListener("click", function() {
  inputBox.classList.toggle("visible");
  document.querySelector("#newTodoLabel").focus();
}, false);
```

This code is listening for when we click the add button. When that happens our `inputBox` variable will have the `.visible` class toggled on; this will reveal the input field (and render a nice transition as well). Afterwards, we select the input field and apply the `.focus()` method... Which focuses it.

Now we need to handle inputting data into the input field. Add this code after the previous block:

```javascript
document.querySelector("#newTodoLabel").addEventListener("keydown", function(event) {
  if (event.keyCode === 13) {
    items.push({ title: event.target.value });
    updateList();
    inputBox.classList.toggle("visible");

    event.target.value = "";
  }
}, false);
```

`event` being passed in as an argument represents the event object. It does not have to be named "event", some other comment names are "e", "element", or "el".

What's happening here is we're checking the `keyCode` property -- which represents the numerical code of a key pressed -- of the event object. If if it is `13` (which is for `enter`), we push an object onto the `items` array. This object has a `title` property that contains the value (`event.target.value`) of what was typed into the input field.

After that, an `updateList();` function is called (we'll define that later) and the `inputBox` has the `.visible` class toggled once again to hide it.

We then clear `e.target.value`.

For reference, `event.target` presents the object that dispatched the event to begin with, this object being `<input type="text" id="newTodoLabel" placeholder="To-do item...">`. The `value` property of `event.target` is the value of whatever we input into that field.

Last but not least, let's define the `updateList()` function. Add this code after the previous block we've written:

```javascript
function updateList() {
  var html     = "";
  var template = document.querySelector("#itemTemplate").innerHTML;

  for (var i = 0; i < items.length; i++) {
    var contents = template.replace("item_id", "item_" + i).replace("item_title", items[i].title);
    html += "<div class='item'>" + contents + "</div>";
  }

  document.querySelector("#itemsList").innerHTML = html;

  [].forEach.call(document.querySelectorAll("input[type=checkbox]"), function (el) {
    el.addEventListener("click", function() {
      el.parentNode.parentNode.classList.toggle("checked");
    }, false);
  });
}
```

We've defined two variables within the scope of this function: `html` and `template`. `html` is merely an empty string for now, and `template` has been assigned the child nodes of `<div id="itemTemplate" class="template item">`.

Next, we loop through each item in the previously declared `items` array and assign a new `contents` variable an updated form of the HTML that's within the `template` variable. Those updates were replacing `{{item_id}}` with `"item_" + i` (which will output as "item_1 ... n") and `{{item_title}}` with the value of `title` at whichever iteration we are at in the loop.

Then we select `<div id="itemsList" class="itemsList">` and update its inner HTML to our newly generated list!

Finally, we loop through each checkbox to add a listener so that whenever they're clicked, a `.checked` class will be toggled on that list item which will add or remove a line through the text.

And with that, we are done!

## Additional resources
[AngularJS](https://angularjs.org/)

[React](https://facebook.github.io/react/)

[SASS](http://sass-lang.com/)

[PostCSS](https://github.com/postcss/postcss)

[Gulp](http://gulpjs.com/)

[Yeoman](http://yeoman.io)
