# REM VS EM

Let's talk about what are REMs and EMs

They are relative units of measurements using in CSS. They can be used for things like width, margin, padding, font-size and more.

PX is an absolute length unit it is fixed and the length expressed will appear at exactly that size.

Using the PX value is not best practice when coding a responsive website. Where as relative lengths specify a length relative to another length property. Relative length units scale better between different rendering mediums and are better for responsive coding.

the EM unit is Relative to the font-size of the element (2em means 2 times the size of the current font)

When using em units, the pixel value you end up with is a multiplication of the font size on the element being styled.

```css
div {
	font-size: 30px;
}

span {
	font-size: 0.5em; /* = 15px */
}

@media screen and (min-width: 375px) {
	div {
		font-size: 15px;
	}
}
```

Using the EM value can get complicated when you start nesting elements. for example if we have a page with a root font size of 16px (usually the default) and a child div inside it with padding of 1.5em, that div will inherit the font size of 16px from the root element. Hence its padding will translate to 24px, i.e. 1.5 x 16 = 24.

```css
.em-example {
	background: #3498db;
	color: #fff;
}

.em-example {
	padding: 1.5em;
}
```

Then what if we wrap another div around the first and set its font-size to 1.25em?

Our wrapper div inherits the root font size of 16px and multiplies that by its own font-size setting of 1.25em. This sets the div to have a font size of 20px, i.e. 1.25 x 16 = 20.

Now our original div is no longer inheriting directly from the root element, instead it’s inheriting a font size of 20px from its new parent div. Its padding value of 1.5em now equates to 30px, i.e. 1.5 x 20 = 30.

```css
.wrapper {
	font-size: 1.25em; /* = 16 x 1.25 = 20px  */
}

.em-example {
	padding: 1.5em; /* = 20 x 1.5 = 30px  */
}
```

Things can get even crazier if you add an em font-size to the original div let's say 1.2em

The div inherits the 20px font size from its parent, then multiplies that by its own 1.2em setting, giving it a new font size of 24px, i.e. 1.2 x 20 = 24.

The 1.5em padding on our div will now change in size again with the new font size, this time to 36px, i.e. 1.5 x 24 = 36.

```css
.wrapper {
	font-size: 1.25em; /* = 16 x 1.25 = 20px  */
}

.em-example {
	font-size: 1.2em; /* = 20 x 1.2 = 24px  */
	padding: 1.5em; /* = 24 x 1.5 = 36px  */
}
```

With all this potential for complication, you can see why its important to know how to use em units in a manageable way.

Now since you are fully confused let's chat about REM units

When using rem units, the pixel size they translate to depends on the font size of the root element of the page, i.e. the html element. That root font size is multiplied by whatever number you’re using with your rem unit.

For example, with a root element font size of 16px, 10rem would equate to 160px, i.e. 10 x 16 = 160.

```css
.rem-example div {
	font-size: 3rem;
	border: 1px solid black;
}

div.rem-example {
	font-size: 2rem;
	border: 1px solid red;
}
```

The difference between `em` and `rem` units is how the browser determines the `px` value they translate into. Understanding this difference is the key to determining when to use each unit.

Okay now since we have an understanding of how EM and REM works, let's talk about how to use them in real world scenarios.

My recommendation is to use REM if you are unsure. Typically EM is going to be used for margin, padding, width, height, and line-height. If you are going to use EM for these I would suggest setting a font-size on the element using REM to avoid inheritance confusion like I showed earlier.

### Wrapping up

    - rem and em units are computed into pixel values by the browser, based on font sizes in your design.
    - em units are based on the font size of the element they’re used on.
    - rem units are based on the font size of the html element.
    - em units can be influenced by font size inheritance from any parent element
    - rem units can be influenced by font size inheritance from browser font settings.
    - Use rem units unless you’re sure you need em units, including on font sizes.
    - Use rem units on media queries

# Parallax Tutorial

There are a number of ways to achieve this feature and W3schools has a very basic implementation:

https://www.w3schools.com/howto/howto_css_parallax.asp

Use a container element and add a background image to the container with a specific height. Then use the background-attachment: fixed to create the actual parallax effect. The other background properties are used to center and scale the image perfectly:

```html
<style>
	body,
	html {
		height: 100%;
	}

	.parallax {
		/* The image used */
		background-image: url("img_parallax.jpg");

		/* Full height */
		height: 100%;

		/* Create the parallax scrolling effect */
		background-attachment: fixed;
		background-position: center;
		background-repeat: no-repeat;
		background-size: cover;
	}
</style>

<!-- Container element -->
<div class="parallax"></div>
```

This simple CSS will achieve the feature but you can add more advanced CSS to make the effect a little more interactive.

Let's take a look at an example using the transform property in combination with a background-image.

First let's build the structure by creating 2 wrapper divs. One for parallax and one for regular. Also added some content inside too.

```html
<div class="navbar"><span>CSS Parallax Scrolling Tutorial</span></div>
<div class="parallax-wrapper">
	<div class="content">
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. .... .... Donec in justo eu ligula semper consequat sed a risus.</p>
	</div>
</div>
<div class="regular-wrapper">
	<div class="content">
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. .... .... Donec in justo eu ligula semper consequat sed a risus.</p>
	</div>
</div>
```

First I am going to add in my style resets then for the CSS we going to set all width and height to 100% of the viewport (100vw and 100vh) and added some padding and background for aesthetic purpose. This will include the padding into the width and height and make our wrapper size exactly at 100% of the viewport.

```css
.parallax-wrapper {
	width: 100vw;
	height: 100vh;
	padding-top: 20vh;
}
.regular-wrapper {
	width: 100vw;
	height: 100vh;
	padding-top: 20vh;
	background-color: aliceblue;
}
.content {
	margin: 0 auto;
	padding: 50px;
	width: 50%;
	background: #aaa;
}
```

Next I’m going add a pseudo element to the parallax wrapper. This element will be served as a background and this is what we are going to add a transform to later. but for now let’s set the size/position and background image first.

```css
.parallax-wrapper::before {
	content: "";
	width: 100vw;
	height: 100vh;
	top: 0;
	left: 0;
	background-image: url(./img/bkg-repeat.jpg);
	position: absolute;
	z-index: -1;
}
```

Note: Adding position absolute is important since we want this element to be in background and out of the way for the content. (Absolute positioned element is removed from the normal document flow)

OK, the structure is complete. Let’s work on the parallax!

First set the overflow hidden to the root element and height of the body to 100% of viewport to fix the viewing area (or we won’t see the parallax effect) Then add a perspective to instruct the browser to display in 3D perspective mode. Also add a preserve-3d transform style to the body and to the parallax wrapper.

```css
html {
	overflow: hidden;
}
body {
	height: 100vh;
	perspective: 1px;
	transform-style: preserve-3d;
	overflow-x: hidden;
	overflow-y: auto;
}
.parallax-wrapper {
	transform-style: preserve-3d;
}
```

I set overflow-x to hidden and overflow-y to auto. This because we’re working in 3D mode and we do not want the horizontal scrollbar to appear for the off screen content.

Next we’re going to push the background div to the back by adding translateZ for -1px. By adding this, the distance between the viewing area and this element becomes farther. As a result, you’ll see this div smaller. Since we set the perspective value to 1px and we also moved the div back for 1px, you’ll see the div at half of the original size. To counter this, we’ll scale the div by 2 times.

```css
.parallax-wrapper::before {
	transform: translateZ(-1px) scale(2);
}
```

If you try scrolling now you’ll see that the regular-wrapper is being overlay by the parallax-wrapper. we can fix this by setting the z-index of the parallax-wrapper to negative and regular to positive. since z-index only works on positioned element, I’m going to also add position relative to the regular wrapper.

```css
.parallax-wrapper::before {
	z-index: -1;
}
.regular-wrapper {
	z-index: 2;
	position: relative;
}
```

And there you have it a completely confusing but accurate way of achieving a parallax scrolling feature.
