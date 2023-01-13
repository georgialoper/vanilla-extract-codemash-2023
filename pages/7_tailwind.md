# Atomic CSS

<!--

Okay, let's talk about Atomic CSS. 
-->

---

#### Atomic CSS

 - ~1:1 class to CSS property
 - Classes are composed together in markup

<!--
Not to be confused with atomic design, atomic CSS as an approach to CSS where one class styles very few, usually one, CSS property.

These single purpose CSS classes are then composed together in the markup in order to supply all the requisite styles for the element.

Tailwind is the dominant framework following this approach, but there are many other similar frameworks out there. Tachyons was popular for a while. Bootstrap provides quite a lot of atomic CSS classes, but does not lean into it quite like Tailwind. 

I've authored custom-implemented atomic CSS in a number of applications, and I'm sure you've at least worked in an application with atomic CSS to some degree.
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
 This is what a static example of our heading component might look like if we fully lean into tailwind

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

<!-- It might even support theming using some CSS variables -->

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

Then, since we're combining a number of classes, we'll likely pull in a classnames library.

click

We'll use that to bind together some classnames, but unless we expect the consumer of this component to pass in the atomic CSS class. We'll have to map the tokens we want to expose to the actual classes.
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
So now even if we're using a framework to provide a lot of our classes, we're still maintaining all these maps.

This might be more or less maintainable depending on how many tokens you have. 

I know Tailwind's framework have IDE integrations that might be helpful for remembering classes there are or generating these maps, but you can't actually use that information from the IDE in code to keep these maps in sync with what classes actually exist.
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
- Reusable CSS helps keep bundle size small
  - define all the things once, use everywhere
	- Author less custom CSS
	- Don’t have to invent classnames
- Performant Static Stylesheet
  - purge CSS
- Don't need a library that runs in the browser
- Make safer changes
  - can you actually?
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
	- Both you and your cookie script are both using differently themed versions of tailwind.
- Mental overhead
- DevX
  - lots to maintain to be able to use of props & state for styling
  - IDE plugins and linters can make it feel type safety, but there’s no way to actually use those types in code
	- Can become unreadable when lots of classes are applied (I don't think this is actually a problem in the browser, but can be in reviewing code)
-->