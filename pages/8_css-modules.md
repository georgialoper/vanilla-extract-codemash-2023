# CSS Modules

<!-- Let's round out our evaluations by taking a look at CSS-modules -->

---

#### CSS Modules

- CSS-like syntax
- Build time CSS-in-JS

<!--
Written like normal CSS but is actually build time CSS in JS.
The advantage of this is that it can be imported as a module into your components and it uses a build step to scope your styles specifically to that component -->

---

#### CSS Modules

```css
/* Heading.module.scss */
.root {
	background: "navy";
	color: "plum";
	padding: 1rem;
}
```

<!--
Fist we'll define our styles in a .module.scss file.
Looks and feels just like we're writing plain 'ol CSS.
-->

---

#### CSS Modules

```jsx {all|2|5}
// Heading.tsx
import styles from './Heading.module.css'

export const Heading = ({ children }) => (
	<div className={ styles.root }>
		<h1>{ className }</h1>
	</div>
)
```

<!-- Then we'll go over to our Heading component

click

and import the styles we defined in our .module.scss file as a JavaScript module.

click

Then we'll apply our root class we defined by keying the root off of the styles module we imported -->

---

#### CSS Modules

```css
.Heading-root-cnq0T {
	background: "navy";
	color: "plum";
	padding: 1rem;
}
```

<!-- Then, if we look at the CSS in the browser it looks like this. First, you'll notice that it's hashed, so we know it's scoped to the element that we explicitly assigned it to.

However, unlike with runtime CSS-in-JS, this CSS is actually built with the application and sent to the browser as a static CSS file. -->

---
layout: two-cols
---

#### CSS Modules

# 👍

::right::

 - Locally scoped
 - Explicit dependencies
 - Familiar syntax

<!-- 
pros
- Locally-scoped
- Explicit dependencies
    - Can be tree-shaken
- looks and feels like regular CSS
-->

---
layout: two-cols
---

#### CSS Modules

# 👎

::right::

 - DevX
 - Runtime theming
 - Typesafety

<!--

cons
- DevX - No great path forward to use props & state for styling in a way that’s reusable
  - often end up recreating globally-scoped atomic CSS
- Doesn't readily support Runtime theming: Even scss compiles down to the values
  - have to create my own css variable layer in my sass token layer
- no type safety
-->