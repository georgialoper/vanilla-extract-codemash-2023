# Atomic CSS

<!--

Okay, let's talk about Atomic CSS. 
-->

---

#### Atomic CSS

 - ~1:1 class to CSS property
 - Classes are composed together in markup

<!--
Not to be confused with atomic design, atomic CSS as an approach to CSS where one class styles very few, usually one, CSS properties.

These single purpose CSS classes are then composed together in the markup in order to supply all the requisite styles for the element.

Like I said, there are many frameworks and in-house solutions that would be considered atomic CSS. but
Tailwind is the dominant framework following this approach, so the examples we look at today will be using tailwind.
-->

---

#### Atomic CSS

```jsx {all|2}
export const Heading = ({ children }) => (
	<div className="bg-primary text-secondary p-4">
		<h1>{ children}</h1>
	</div>
)
```

<!--
 This is what a static example of our heading component might look like

 click

 You can see we are applying 3 classes, one for each property we are styling
 
 -->

---

#### Atomic CSS

```css
.bg-primary {
	background: "navy";
}

.text-secondary {
	color: "plum";
}

.p-4 {
	padding: 1rem;
}
```

<!-- This is what the CSS might look like. Unlike CSS in JS this would be sent down to the browser as a static CSS file. -->

---

#### Atomic CSS

```css
.bg-primary {
	background: var(--color-primary);
}

.text-secondary {
	color: var(--color-secondary);
}

.p-4 {
	padding: 1rem;
}

:root {
	--color-primary: "navy";
  --color-secondary: "plum";
}
```

<!-- It might even be setup to support theming using CSS variables -->

---

#### Atomic CSS

```jsx {all|3|1|5-9}
import cx from 'classnames'

export const Heading = ({ background, color, space, children }) => (
	<div
		className={cx(
			backgroundMap[background],
			colorMap[color],
			spaceMap[space]
		)}
	>
		<h1>{ children }</h1>
	</div>
)
```

<!-- 
Now let's try to dynamically style our component based on the props.

click

We'll go ahead and expose those on our component.

click

Then, since we're combining a number of classes, we'll likely pull in a classnames library. Probably not super necessary for this example, but this would definetly be useful for elements that need more properties styled.

click

We'll use that to bind together our classnames. Now unless we expect the consumer of this component to pass in the actual atomic CSS classes as props. We'll have to map the tokens we want to expose to the actual underlying classes.
-->

---

#### Atomic CSS

```js
const backgroundMap = {
	primary: "bg-primary",
	secondary: "bg-secondary",
	tertiary: "bg-tertiary",
	...
}

const colorMap = {
	...
}

const spaceMap = {
	...
}
```

<!--
So now even if we're using a framework to provide all our classes, we're still maintaining all these maps.

This might be more or less maintainable depending on how many tokens you have. 

I know Tailwind has IDE integrations that might be helpful for remembering what classes there are or generating these maps, but you can't actually use that information from the IDE in code to keep these maps in sync with what classes actually exist.
-->

---
layout: two-cols
---

#### Atomic CSS

# 👍

::right::

 - Finite CSS
 - Static Stylesheet
 - Bundle size

<!-- 
pros of atomic CSS 
- Finite amount of CSS in your app
  - define all the things once, reusing  everywhere
	- Author less custom CSS
	- Don’t have to invent classnames
- Performance benefits of a Static Stylesheet
  - Additionally, Tailwind offers a purge CSS build plugin will remove all classes from your static CSS that aren't used in your page or app
- Bundle size: Don't need to send down a library that runs stuff in the browser
- Some claim tailwind allows you to make changes more safely, since following this approach you should you change your markup rather than touching your CSS
  - I'm not convinced that's a legit point. That's a much larger refactor that I'd hope to take on when I have tokenized CSS 
-->

---
layout: two-cols
---

#### Atomic CSS

# 👎

::right::

 - Global scope
 - Mental overhead
 - DevX

<!--

cons
- Global scope
	- Not hashed. If both you and your injected cookie consent UI are both using differently overlapping atomic CSS classes. 
- Mental overhead
- DevX
  - lots to maintain to be able to use of props & state for styling
  - IDE plugins and linters can make it feel type safety, but there’s no way to actually use those types in code
	- Can become unreadable when lots of classes are applied (I don't think this is actually a problem in the browser, just some strong opinions)
-->