
## Runtime CSS-in-JS

<!-- What even is Runtime CSS-in-JS? -->

---

#### Runtime CSS-in-JS

 - Author styles in JS or TS
 - Styles interpreted and applied at runtime

<!--
As the name suggests, CSS-in-JS allows you to style your components by writing CSS directly in your JavaScript or TypeScript code.

The library then generates the styles dynamically creates the necessary styles at runtime in the browser.

So what does this look like?
-->

---

#### Runtime CSS-in-JS

```jsx {all|4-8}
// @emotion/react (css prop), with object styles
export const Heading = ({ children }) =>(
	<div
		css={{
			background: "navy",
			color: "plum",
			padding: "1rem"
    }}
	>
		<h1>{ children }</h1>
	</div>
)
```

<!--

This is a representative example of what our Heading component would look like with just our basic set of styles.

For this example I'm using the emotion react library, which would require some setup in our application's build process. 

click

The library might expose this css props on all our elements, or we might otherwise import it as a function from the library, the result of which we'd just pass on to the elements class prop. 

Regardless, we'll supply our styles to the css prop or function as an object that's representative of the styles we want to apply to our div element.

-->

---

#### Runtime CSS-in-JS

```jsx {all|1|3-7}
export const Heading = ({ background, color, padding, children }) => (
	<div
		css={{
			background,
			color,
			padding
    }}
	>
		<h1>{ children }</h1>
	</div>
)
```
<!-- 
You can see how this object in our JSX component lends itself really nicely to allow us to 

click

surface those css properties as props on our component, 

click

and pipe those values directly to our css object.
Since we don't have to do any mapping or anything fancy, or even be aware of any classes, you can see how this provides a very nice devX.

-->

---

```css {all|1}
.css-asdfgh {
	background: "navy";
	color: "plum";
	space: "1rem";
}
```

<!--
Ultimately, this is what the generated css might look like in the browser.

Effectively, the css prop or function from these libraries generates the requested CSS and with and returns the classname which is then applied to the element.

click

Notice the classname contains this hash, this means that this class will only apply to the element that it's meant for (that called the css function). Uou don't have to worry about mistyping anything and getting a different similarly named class, or collisions from using the same class names as anything you might not have control over that may be inserted into your document. If you have like a cookie banner or something.
-->

---
layout: two-cols
---

#### Runtime CSS-in-JS

# 👍

::right::

 - DevX
 - Co-location
 - Locally scoped
 - *Can* be made typesafe

<!--
What are the pros of Runtime CSS in JS?

 - DX: use of JS to define styles
    - Reduces duplication
    - Use props & states without inlining
      - inlining is not ideal for performance when the same styles are applied to many elements
 - Co-location
   - Prevents dead code since the CSS is likely to be deleted with when the elements that use it are deleted
 - Locally scoped
   - Styles meant for a specific element won't have unintended affects on other elements
 - *Can* be made typesafe
-->

---
layout: two-cols
---


#### Runtime CSS-in-JS

# 👎

::right::

 - Runtime overhead
 - SSR complexity
 - Bundle size

<!-- 
What are the cons of Runtime CSS-in-JS?
Turns out generating all your styles in the browser at runtime is expensive.

Additionally, since the creation of styles happen in the browser, it's non-trivial to SSR pages while preventing layout shift

And, of course, your chosen CSS in JS library needs to be sent to the browser to perform all this, increasing your bundle size.  
-->

---

<img src="/assets/why-were-breaking-up-with-css-in-js.png"/>

<!-- If you're interested in more details on this, highly recommended checking out this article that was circling around twitter a couple months ago. Though, it does errantly lump vanilla-extract in with these runtime css in js libraries. -->

