# CSS Fundamentals

## Box Model

### What's the CSS box model hierarchy?

![Box model](http://i.imgur.com/1rr9KWu.png)

### What does the `box-sizing` property do?

The box model in CSS works like this:

* actual visible/rendered width of box = width + padding + border
* actual visible/rendered height of box = height + padding + border

It's a little weird, but you get used to it. This is called `box-sizing: content-box` which is the default value when it comes to calculating the dimensions of a box.

If we set the `box-sizing` property to `border-box`, the box model is treated differently.

* actual visible/rendered width of box = width
* actual visible/rendered height of box = height

The border and padding values move inward, cutting into the width/height of the box rather than expanding it.

So, if we want to create a selector to target all `.little-box` elements and make them have a total width of 120px with a 20px left and right padding without using the `box-sizing` property, we'd need to do something like this:

```css
.little-box {
  width: 80px;
  padding-left: 20px;
  padding-right: 20px;
}
```

Look how this changes when we set the `box-sizing` property to `border-box`:

```css
.little-box {
  box-sizing: border-box;
  width: 120px;
  padding-left: 20px;
  padding-right: 20px;
}
```

The `border-box` value (as opposed to the `content-box` default) makes the final rendered box the declared width, and any border and padding cut inside the box. This allows us to set the actual, final width without caring for calculations. Now the width (120px) includes both side paddings. What has changed now is the content width, which is the diference between the outer width (120px) and both paddings (40px).

### What's the difference between `width: auto` and `width: 100%`?

`width: auto` will try as hard as possible to keep an element the same width as it's parent container when additional space is added from margins, padding, or borders.

`width: 100%` will make the element as wide as the parent container. Extra spacing will be added to the elements size without regards to the parent. This typically causes problems.

### What are `max-width` and `min-width` good for?

An element to which a `max-width` is applied will never be wider than the value specified even if the width property is set to be wider. `max-width` is often used in conjunction with `min-width` to produce a width range for the element concerned.

Note that `max-width` and `width` shouldn't be applied to the same element using the same unit, as one will override the other. If, for example, the `width` is set to 150px and the `max-width` is set to 60px, the actual width of the element will be 60px, and the `width` declaration will become redundant. Use percentages for the `width` value instead.

![Width auto vs 100%](http://i.imgur.com/4RLTge7.jpg)

## Element Positioning

### What are the main `display` values an element can take?

* **Inline**

`display: inline` is the default value for all elements. Think of elements like `span`, `em`, or `b` and how wrapping text in those elements within a string of text doesn't break the flow of the text. An inline element will accept margin and padding, but the element still sits inline as you might expect. Margin and padding will only push other elements horizontally away, not vertically. An inline element will not accept height and width, it will just ignore them.

![inline](http://i.imgur.com/PwpcUti.png)

* **Inline Block**

An element set to `display: inline-block` is very similar to inline in that it will set inline with the natural flow of text (on the "baseline"). The difference is that you are now able to set a width and height which will be respected.

![inline-block](http://i.imgur.com/ePWVIPY.png)

* **Block**

A block-level element (`display: block`) starts on a new line and stretches out to the left and right as far as it can (just like a `div` or `p` do).

More info on table, grid and flex values [here](https://css-tricks.com/almanac/properties/d/display/).

### Difference between `display: none` and `visibility: hidden`

Setting display to none will render the page as though the element does not exist (although you can still interact with it through the DOM). On the other hand, setting visibility to hidden will hide the element, but it will still take up the space it would if it was fully visible.

### What are the different `position` values an element can take?

* **Static**

`position: static` is the default value. An element with `position: static` is not positioned in any special way.

* **Relative**

`position: relative` behaves the same as static unless you add some extra properties. Setting the `top`, `right`, `bottom` and `left` properties of a relatively-positioned element will cause it to be adjusted away from its normal position. This means that the new position (determined by `top`, `right`, `bottom`, `left`) is relative to the original (static) position.

```css
#something {
  position: relative; /* i'm gonna move this element from its original spot */
  top: -10px; /* push this 10px up to the top */
  left: 20px; /* push this 20px to the right */
}
```

* **Fixed**

A fixed element is positioned relative to the viewport, which means it always stays in the same place even if the page is scrolled (think of a modal window). As with relative, the `top`, `right`, `bottom`, and `left` properties are used.

* **Absolute**

`position: absolute` behaves like `fixed` except relative to the nearest positioned ancestor instead of relative to the viewport. If an absolutely-positioned element has no positioned ancestors, it uses the document body, and still moves along with page scrolling. Remember, a "positioned" element is one whose position is anything except `static`.

### How does `z-index` work?

The z-index property in CSS controls the vertical stacking order of elements that overlap. As in, which one appears as if it is physically closer to you. `z-index` only effects elements that have a position value other than static (the default).

Elements can overlap for a variety of reasons, for instance relative positioning has nudged it over something else. Negative margin has pulled the element over another. Absolutely positioned elements overlap each other. All sorts of reasons. Without any z-index value, elements stack in the order that they appear in the DOM (the lowest one down at the same hierarchy level appears on top). Elements with non-static positioning will always appear on top of elements with default static positioning.

### What does `margin: auto` do?

You can set the left and right margins to `auto` to horizontally center an element within its container.

### How to vertically align elements in CSS?

Tough one! Take a look at [this article](https://css-tricks.com/centering-in-the-unknown/) and also [this one](http://howtocenterincss.com/).

### How does floating elements work?

Our HTML is bound by some rules, in particular, the normal flow. In the normal flow, each block element (`div`, `p`, `h1`, etc.) stacks on top of each other vertically, from the top of the viewport down. Floated elements are first laid out according to the normal flow, then taken out of the normal flow and sent as far to the right or left (depending on which value is applied) of the parent element. In other words, they go from stacking on top of each other to sitting next to each other, given that there is enough room in the parent element for each floated element to sit.

Notice that depending on the size of the container (parent), the floated (children) elements will drop to a second row when there is not enough room for all of them to sit side by side.

### What does the `clear` property do?

The clear property has five values available: `left`, `right`, `both`, `inherit`, and `none`. Assigning a value of `left` says the top edge of this element must sit below any element that has the `float: left` property applied to it. The same concept applies for the `right` value: the element must sit beneath any element that has the `float: right` property applied to it. Using the `both` value tells our element that its top edge must sit below any element floated either left or right. The inherit value takes on the clear property from its parent element, while the default value `none` behaves as you would expect.

This is an immensely powerful property; as you can see, it helps bring our non-floated elements back into the normal flow, a behavior that we tend to expect by default.

### Why is the `float` property important? What other alternative do we have for bulding grid layouts?

The CSS float property allows you to incorporate table-like columns in an HTML layout without the use of tables. If it were not for the CSS float property, CSS layouts would not be possible except using absolute and relative positioning — which would be messy and would make the layout unmaintainable.

### How to fix the collapsed parent effect?

One of the most common symptoms of float-heavy layouts is the "collapsing parent". This is demonstrated in the example below:

![Collapsed Parent](http://i.imgur.com/xXa5jBO.png)

Notice that the bottom of the floated image appears outside its parent. The parent does not fully expand to hold the floated image. This is caused because the floated element is out of the flow in relation to other block elements, so all block elements will render as if the floated element is not even there. This is not a CSS bug, it's in fact in line with CSS specifications.

The easiest way to fix this problem is to float the containing parent element. Now the container expands to fit all the child elements. But unfortunately this fix will only work in a limited number of circumstances, since floating the parent may have undesirable effects on your layout.

Another solution would be to simply add an extra element at the very bottom and "clear" it.

```html
<div id="container">
  <img src="lifesaver.jpg" alt="Lifesaver" />
  <p>Pellentesque habitant morbi tristique senectus...</p>
  <div class="clearfix"></div>
</div>
```

```css
.clearfix {
  clear: both;
}
```

By far the best, and easiest solution to resolve the collapsing parent issue is to add either `overflow: hidden` or `overflow: auto` to the parent element. This is clean, easy to maintain, works in almost all browsers (but IE6) and does not add extra markup.

## Selectors

### What do these selectors do? What's a pseudo class?

Basic selectors:

```css
* {} /* the star symbol will target every single element on the page */
p {} /* all occurrances of a particular tag */
ul li {} /* nested elements (descendants) */
ul > li {} /* direct (immediate) children */
ul#menu {} /* tags with a particular (unique) id */
p.hightligh {} /* tags with a particular (not necessarily unique) class */
h4, p.highlight {} /* grouping */
ul + p {} /* the adjacent selector will target only the first p after each ul */
ul ~ p {} /* any p elements, as long as they follow an ul (not necessarily immediate siblings) */
```

Attribute selectors:

```css
a[title] {} /* this will only select the anchor tags that have a title attribute */
a[href="http://google.com"] {} /* this will style all anchor tags which link to Google */
a[href*="google"] {} /* this covers mail.google.com as well as google.co.nz */
a[href^="http"] {} /* the caret symbol designates the beginning of a string... ever wonder how some websites are able to display a little icon next to the links which are external? */
a[href$=".jpg"] {} /* we use the dollar symbol to refer to the end of a string */
a[data-filetype="image"] {} /* target html elements with that data attribute */
a[data-info~="external"] {} /* target an attribute which has a space-separated list of values */
input[type=radio]:checked {} /* target all checked radio buttons */
```

Conditional selectors:

```css
div:not(#container) {} /* target all divs but the #container */
*:not(p) {} /* target everything but paragraphs */
```

Structural pseudo classes:

```css
p::first-line {}
p::first-letter {}
li:first-child, li:last-child {}
li:nth-child(3) {} /* NOT zero based! */
li:nth-child(even) {}
li:nth-child(3n) {} /* target all elements whose index is a multiple of 3 */
li:only-child {} /* target elements which are the only child of its parent */
```

Learn more about CSS selectors and pseudo-selectors [here](http://code.tutsplus.com/tutorials/the-30-css-selectors-you-must-memorize--net-16048). It would also be a good idea to make candidates play [this game](http://flukeout.github.io/) in order to test their understanding on CSS selectors.

### Difference between `:nth-child` and `:nth-of-type`

The `:nth-child` selector means select an element provided it's a paragraph AND ALSO the second child of a parent. On the other hand, the `:nth-of-type` selector means select the second paragraph child of a parent (BUT it needn't be the second child ever, this is "less conditional"). See [this article](https://css-tricks.com/the-difference-between-nth-child-and-nth-of-type/).

### Why are `::before` and `::after` useful?

These are pseudo elements which allows you to insert content onto a page from CSS (without it needing to be in the HTML). Note that **the end result is not actually in the DOM**. Check [this blog entry](http://krasimirtsonev.com/blog/article/CSS-before-and-after-pseudo-elements-in-practice) to see some ideas on how to use these pseudo elements.

Note that self-closing elements such as <img />, <input /> and <hr /> can't have pseudo-elements attached to them.

### `:` vs `::`

Every browser that supports the double colon (`::`) CSS3 syntax also supports just the (`:`) syntax, but IE 8 only supports the single-colon, so for now, it's recommended to just use the single-colon for best browser support. `::` is the newer format indented to distinguish pseudo content from pseudo selectors. Pseudo elements (or pseudo content) create new virtual elements, whereas pseudo classes just act as ordinary selectors.

## Units

### What CSS length units are you familiar with?

* **Absolute Lengths**
  * `px` (pixels)
  * `in` (inches) → 1in = 96px
  * `cm` (centimeters) → 1cm = 37.8px
  * `mm` (milimiters) → 1mm = 0.1cm = 3.78px
* **Font-Relative Lengths**
  * `em` → without any CSS at all, 1em would be 16px, but if any CSS changes the font size (at any level in the document) 1em becomes whatever the new font-size is
  * `rem` → always relative to the root element (`:root {}`) rather than cascading like `em` does
  * `pt` (point) → 1pt = 1/72 of an inch
  * `pc` (pica) → 1pc = 12pt
  * `ex` → based on the x-height of the current font, and it changes as the font-family changes
  * `ch` → based on the width of the zero (0) character instead of the height of the x character, and it also changes as the font-family changes
* **Viewport Percentage Lengths**
  * `vw` (viewport width) → 1vw is equal to 1% of the width of the viewport
  * `vh` (viewport height) → 1vh is equal to 1% of the height of the viewport
  * `vmin` → this value will be whichever is smaller at the moment, vw or vh
  * `vmax` → this value will be whichever is larger at the moment, vw or vh
* **Odd Ball Out**
  * `%` (percentage) → based on the length of same property of the parent element

More info on this [here](https://css-tricks.com/the-lengths-of-css/) and also [here](http://webdesign.tutsplus.com/articles/7-css-units-you-might-not-know-about--cms-22573).

### What's the difference between `em` and `rem`?

Both `rem` and `em` are relative units, `px` is not. Below, the example demonstrates how each nested child assumes the parent is 1em (100%). Thus children inherit size by scaling in relation to the parent font size. While the value remains 0.773em, the actual font size is calculated at 77.3% of its direct parent, which in turn scales from its parent. This continues up the DOM tree whenever a parent has a defined `font-size`.

![em vs px](http://i.imgur.com/HTUzG2Z.png)

While `em` is relative to the `font-size` of its direct or nearest parent, `rem` is only relative to the `html` (root) `font-size`. If you're trying to achieve consistent spacing without extra markup, `rem` can be used to define the padding and margins. `px` can be used for consistent padding and margins too, but it doesn't scale across media queries like `em` and `rem`.

![em vs rem][Imgur](http://i.imgur.com/VH8LDNr.png)

More info on this [here](https://j.eremy.net/confused-about-rem-and-em/).

## Filters, Transformations and Transitions

### What are CSS filters?

The CSS filter property provides access to effects like blur or color shifting on an element's rendering before the element is displayed.

* `filter: blur(20px) grayscale(20%)` (example of multiple filters being used)
* `filter: sepia(1)`
* `filter: saturate(8)`
* `filter: hue-rotate(90deg)`
* `filter: invert(.8)`
* `filter: opacity(.2)`
* `filter: brightness(3)`
* `filter: contrast(4)`

### What are CSS transformations?

The CSS transform property allows you to visually manipulate element, literally transforming their appearance.

* `transform: translate(50px, 100px)` (there's also `translateX()` and `translateY()`)
* `transform: rotate(20deg)`
* `transform: scale(2, 3)` (there's also `scaleX()` and `scaleY()`)
* `transform: skew(20deg, 10deg)` (there's also `skewX()` and `skewY()`)
* `transform: matrix(1, -0.3, 0, 1, 0, 0)` (combines all the 2D transform methods into one)

These just made 2D transformations, but there are also 3D transformation methods:

* `transform: rotateX(150deg)` (rotates an element around its X-axis at a given degree)
* `transform: rotateY(130deg)` (rotates an element around its Y-axis at a given degree)
* `transform: rotateX(90deg)` (rotates an element around its Z-axis at a given degree)

More info on CSS3 transformations [here](https://css-tricks.com/almanac/properties/t/transform/).

### What are CSS transitions?

They allow elements to change values over a specified duration, animating the property changes, rather than having them occur immediately.

```css
div {
  transition: background-color 0.5s ease;
  background-color: orange;
}

div:hover {
  background-color: green;
}
```

Here's an overview of the syntax:

```css
/* Apply to 1 property */
/* property name | duration */
transition: margin-left 4s;

/* property name | duration | delay */
transition: margin-left 4s 1s;

/* property name | duration | timing function | delay */
transition: margin-left 4s ease-in-out 1s;

/* Apply to 2 properties */
transition: margin-left 4s, color 1s;

/* Apply to all changed properties */
transition: all 0.5s ease-out;
```

Most common timing functions are `ease`, `linear`, `ease-in`, `ease-out`, `ease-in-out`, `step-start`, `step-end`. Check out [this link](https://css-tricks.com/almanac/properties/t/transition-timing-function/) for more info on timing functions.

[This](http://webdesign.tutsplus.com/articles/css3-transitions-and-transforms-from-scratch--webdesign-4975) is a great resource to learn about both transitions and transforms.

### What is specificity? How do you calculate specificity?

The different weight of selectors is usually the reason why your CSS rules don't apply to some elements, although you think they should have. I

* There are four distinct categories which define the specificity level of a given selector: inline styles, IDs, classes+attributes and elements.
* When selectors have an equal specificity value, the latest rule is the one that counts.
* Rules with more specific selectors have a greater specificity.
* The last rule defined overrides any previous, conflicting rules.
* You should always try to use IDs to increase the specificity.
* A class selector beats any number of element selectors.

You can calculate CSS specificity using the [CSS Specificity Calculator](http://specificity.keegan.st/).

For more info and examples check the [CSS Specificity Wars](http://www.stuffandnonsense.co.uk/archives/css_specificity_wars.html).

## Misc

### Styling Placeholders

`:placeholder-shown` is used to select the input field itself when its placeholder is being shown. `::placeholder`, on the other hand, styles the placeholder text.

[Imgur](https://i.imgur.com/yLR14ZA.png)

## Responsive Web Design (RWD)

### What's RWD?

Read Ethan Marcotte's [original definition](http://alistapart.com/article/responsive-web-design/).

RWD foundations for crafting multi-device web experiences are:

* fluid grids
* flexible media
* media queries

Responsive web design does not need to stop at making squishy layouts. This isn't a one-size-fits-all solution. [Responsive design is very much an extension of progressive enhancement](http://bradfrost.com/blog/post/the-principles-of-adaptive-design/), so don't get hung up thinking that media queries are the only tool in your toolbox:

* Recognize that the Web is a continuum, not a platform.
* Use feature detection to capitalize on device/browser capabilities so that we can support more devices while still optimizing for the the best of the best.
* No one knows what the Web and device landscape is going to look like in a couple years, but there's a good chance the gadgets sitting underneath Christmas trees a few years from now will have access to the Web. The key aspect of future-friendly thinking is to acknowledge and embrace unpredictability. Think in a future-friendly way by focusing on what really matters, creating portable data, and getting your content ready to go anywhere.
* Be present friendly. When it comes to the Web, the more backward-compatible you are, the more forward-compatible you're likely to be.
* Don't just create myopic buckets ("phone", "tablet", and "desktop"). Rather consider the entire resolution spectrum and all the devices–both present and future–that will access your experience.
* Embrace conditional loading, as this crucial technique helps us deliver highly-performant and fully-featured experiences to our users.
* Get into the browser sooner and test early and often. Performance is invisible, so testing is essential to help us catch performance-hurting decisions.

### Responsive vs Adaptive web design

**Responsive Web Design** provides the optimal viewing experience of a website, no matter what type of device the user is seeing it on. Wikipedia describes it as "an approach aimed at crafting sites to provide an optimal viewing experience (easy reading and navigation with a minimum of resizing, panning, and scrolling) across a wide range of devices from mobile phones to desktop computer monitors". This is done by using fluid grids, which is a term for a design that works no matter what the screen size is. So no matter how much you resize the screen, that same layout will automatically respond to that size.

**Adaptive Web Design** is different from Responsive Design in that there isn't one layout that always changes. Instead, there are several distinct layouts for multiple screen sizes, and the layout used depends on the screen size used. For example, there could be a specific layout for mobile phones, tablets, and desktop computers – each of which are made in advance. These three designs wait on standby until someone visits the site; the site detects the type of device used, and delivers the pre-set layout for that device.

* *Responsive is Harder to Make*: Responsive design is the most difficult choice to pull off since it requires extra attention to the site's CSS and organization to make sure it works well at **any** possible size. It's easier to make a few specific layouts for your website instead of making one layout that can work in any screen size. While this means that each Adaptive layout needs a bit of flexibility to work on several screen sizes, it is easier than making one layout that works for all of them.

* *Adaptive is Less Flexible:* As the last point showed, one drawback of Adaptive design's easier approach is that the final results don't always display the best for a wide variety of screen sizes. While Responsive site designs are guaranteed to work well on any screen size, including new ones continually coming out, Adaptive designs only work on as many screens as its layouts are able to. So if a new device with a new screen size is released, you may find out that none of your Adaptive layouts fit with it well, so you'll have to edit them or add a new one. Responsive sites are flexible enough to keep working on their own, but Adaptive sites will likely need some occasional maintenance.

### What's a media query?

Conditionals... that's what media queries are: logical if statements. If these things are true about the browser, use the CSS inside.

```css
@media (min-width: 600px) and (max-width: 800px) {
  .something { background: red; }
}
```

More info on media queries [here](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Media_queries) and also [here](https://css-tricks.com/logic-in-media-queries/).

### What do mobile-first and desktop-first mean? How'd you implement either approach?

For mobile first, your small screen styles are in your regular screen CSS and then as the screen gets larger you override what you need to. That's why we use `min-width` media queries in general.

```css
html { background: red; }

@media (min-width: 600px) {
  html { background: green; }
}
```

On the other hand, for desktop first your large screen styles are in your regular screen CSS and then as the screen gets smaller you override what you need to. So we need to use `max-width` media queries here.

```css
html { background: red; }

@media (max-width: 600px) {
  html { background: green; }
}
```

## CSS Tools and Workflows

### What's a CSS pre-processor?

A CSS pre-processor extends the CSS language, adding features that allow **variables**, **mixins**, **functions** and many other techniques that allow you to make CSS that is more maintainable, themable and extendable. Most popular ones are [Less](http://lesscss.org/), [Sass](http://sass-lang.com/) and [Stylus](https://learnboost.github.io/stylus/).

### What's Compass?

[Compass](http://compass-style.org/) is an extension of Sass (as in Compass requires Sass). It has its own compiler and a large collection of mixins and functions that are incredibly useful (while commonly pointed to for generating vendor prefixed CSS3 properties, it can do things like automatically generate spritemaps and the CSS to go with them).

### What's Lesshat?

[Lesshat](http://lesshat.madebysource.com/) is a mixin library for Less.

### What's a CSS Reset?

A CSS Reset is a short set of CSS rules that resets the styling of all HTML elements to a consistent baseline. Every browser has its own default ‘user agent' stylesheet, that it uses to make unstyled websites appear more legible. For example, most browsers by default make links blue and visited links purple, give tables a certain amount of border and padding, apply variable font-sizes to H1, H2, H3 etc. and a certain amount of padding to almost everything. Ever wondered why Submit buttons look different in every browser? Obviously this creates a certain amount of headaches for CSS authors, who can't work out how to make their websites look the same in every browser. Using a CSS Reset, CSS authors can force every browser to have all its styles reset to null, thus avoiding cross-browser differences as much as possible.

### Ever used Normalize.css? How is it different from a CSS Reset?

Resets impose a homogeneous visual style by flattening the default styles for almost all elements. In contrast, normalize.css retains and preserves many useful default browser styles. This means that you don't have to redeclare styles for all the common typographic elements. When an element has different default styles in different browsers, normalize.css aims to make those styles consistent and in line with modern standards when possible.

### Why use CSS sprites?

It may seem counter intuitive to cram smaller images into a larger image. Wouldn't larger images take longer to load? While the total image size (sometimes) goes up with sprites, several images are loaded with a single HTTP request. Browsers limit the number of concurrent requests a site can make and HTTP requests require a bit of handshaking. Thus, sprites are important for the same reasons that minifying and concatinating CSS and JavaScript are important.

The correct image in the map is loaded using the `background-position` property.

[css-sprite](https://github.com/aslansky/css-sprite) is a wonderful node package that creates sprites from a glob of images. You can also [generate sprites using Compass](http://compass-style.org/help/tutorials/spriting/).

### How can you ensure cross-browser compatibility?

Check out [this article](http://www.smashingmagazine.com/2010/06/07/the-principles-of-cross-browser-css-coding/).

### Why are there vendor prefixes?

The short answer is that CSS3 is not a finished product. CSS 2.1 wasn't published until 2011, and only a handful of CSS3 modules have even reached the level of a formal recommendation, so it's premature to talk about a CSS3 specification that is universally implemented. Thankfully, modern browser makers are helping to push things forward and are helping to craft the standard which follows the lead of real-world practice. As they move closer to general standards, the prefixes are dropped.

### What's a CSS Linter?

Getting our code reviewed by a pro is a great way of improving code quality but what happens if you don't have access to a rockstar programmer? You do the next best thing and grab a 'lint' for that language. Basically, a lint looks at our code and checks for bad programming practices. Undefined variables, inefficient constructs, things like that. Probably the most famous one out there is [CSSLint](http://csslint.net/) which is an open source tool that checks your syntax against a set of predefined rules to root out possible inefficiencies and make sure that your presentation works as expected with little surprises.

## Organising CSS

### OOCSS

Object oriented CSS is a methodology for writing reusable, scalable and maintainable CSS code. It's done by adhering to two main principles: separating *structure from skin* and *container from content*.

More info and examples of OOCSS [here](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/) and also [here](http://appendto.com/2014/04/oocss/).

#### What's a CSS Object?

A CSS object is a repeating visual pattern that can be abstracted into an independent snippet of HTML, CSS, and possibly JavaScript. That object can then be reused throughout a site. In layman's terms, objects in front-end development are simply HTML elements. The CSS is where we make those objects, or elements, modular and location independent to be able to place them anywhere on a page and behave predictably.

#### How do you separate the structure from the skin?

This means separating positioning (`position`, `float`, `margin`, etc.) from styling (`background`, `color`, `border`, etc.). In practice, this essentially means not to mix structure / positioning properties with skin / styling properties on the same class definition.

For instance, instead of this:

```css
#button {
  width: 200px;
  height: 50px;
  padding: 10px;
  border: solid 1px #ccc;
  background: linear-gradient(#ccc, #222);
  box-shadow: rgba(0, 0, 0, .5) 2px 2px 5px;
}

#box {
  width: 400px;
  overflow: hidden;
  border: solid 1px #ccc;
  background: linear-gradient(#ccc, #222);
  box-shadow: rgba(0, 0, 0, .5) 2px 2px 5px;
}

#widget {
  width: 500px;
  min-height: 200px;
  overflow: auto;
  border: solid 1px #ccc;
  background: linear-gradient(#ccc, #222);
  box-shadow: rgba(0, 0, 0, .5) 2px 2px 5px;
}
```

we could have this:

```css
.button {
  width: 200px;
  height: 50px;
}

.box {
  width: 400px;
  overflow: hidden;
}

.widget {
  width: 500px;
  min-height: 200px;
  overflow: auto;
}

.skin {
  border: solid 1px #ccc;
  background: linear-gradient(#ccc, #222);
  box-shadow: rgba(0, 0, 0, .5) 2px 2px 5px;
}
```

Now all the elements are using classes, the common styles are combined into a reusable *skin* and nothing is unnecessarily repeated. We just need to apply this skin class to all the elements and the result will be the same as what the first example would produce, except with less code and a possiblity for further reuse.

#### How do you separate the container from the content?

Separating container from content is done to allow the re-use of elements and classes no matter where you are in the DOM. A styled element should never be dependent on where it's at in a page. For instance, a latest news module found in a sidebar should not be defined by its current place in the sidebar. It should be movable to a main content area, another module, the footer, so on and so forth.

### What is SMACSS?

SMACSS stands for Scalable and Modular Architecture for CSS. It's a book and a methodology for writing CSS, but its most significant and influential aspect is its organizational system, which is designed to provide a set of buckets into which CSS should be organized. To learn more, check out the [SMACSS web site](https://smacss.com/) and read the book there.

### What's BEM?

BEM – meaning block, element, modifier – is a [front-end naming methodology](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/), a smart way of naming your CSS classes to give them more transparency and meaning to other developers. The naming convention follows this pattern:

* `.block` represents the higher level of an abstraction or component.
* `.block__element` represents a descendent of .block that helps form `.block` as a whole.
* `.block--modifier` represents a different state or version of `.block`.

The reason for double rather than single hyphens and underscores is so that your block itself can be hyphen delimited.

Take this searchform example:

```html
<form class="site-search  full">
    <input type="text" class="field">
    <input type="Submit" value ="Search" class="button">
</form>
```

These classes are fairly loose, and don't tell us much. Even though we can work it out, they're very inexplicit. With BEM notation we would now have:

```html
<form class="site-search  site-search--full">
    <input type="text" class="site-search__field">
    <input type="Submit" value ="Search" class="site-search__button">
</form>
```

We can see that we have a block called `.site-search` which has an element which lives inside it called `.site-search__field`. We can also see that there is a variation of the .site-search called `.site-search--full`. We could even have a christmas variation for this searchform called `.site-search--xmas` to use only on Dec 25th.

When you are using BEM, though, it is important to remember that you don’t need to use it for everything. Take for example:

```css
.caps { text-transform: uppercase; }
```

This CSS would never fall into any BEM category, it’s merely a standalone rule.

### Can we use them all together?

Since OOCSS is an abstract coding methodology, BEM is a concrete application of OOCSS, and SMACSS is an OOCSS-focused organizational structure, they actually play together very nicely, especially when you throw Sass into the mix. Read [this article](https://mattstauffer.co/blog/organizing-css-oocss-smacss-and-bem) for more info on this.

### CSS Modules

```
@TODO
```
