
<img src="/assets/header-component-example-base.png" />

<!--
To context set and make sure we're comparing apples to apples, when we look at these approaches, we're going to be examining this heading component.

This is what it looks like visually.
-->

---

#### Markup 

```html
<div>
  <h1>🧁 Hello, CodeMash!</h1>
</div>
```

<!-- This is the markup we'll be aiming for where we have a root div containing an h1 with some content. -->

---

#### Styles

Static

```css
{
  background: "navy";
  color: "plum";
  padding: 1rem;
}
```

<!-- The static version of this component will be super simple. We have a navy background, plum color text, and some padding. -->

---

#### Parent component

Static

```tsx
// App.tsx
<Heading>
  🧁 Hello, CodeMash!
</Heading>
```
<!-- 
We'll encapsulate all this into a Heading component. 

which we will use in our parent component, in this case app.tsx, like this with no additional props. -->

---

#### Parent component

Dynamic

```tsx
// App.tsx
<Heading background="secondary" color="primary" padding="m" >
  🧁 Hello, CodeMash!
</Heading>
```

<!-- We might build up our heading component to accept some number of props to dynamically change it's appearance based on the token values supplied by the parent component like this. -->

---

#### Styles

Themed

```css
{
  background: var(--color-primary);
  color: var(--color-secondary);
  padding: var(--space-small);
}

:root {
	--color-primary: "navy";
  --color-secondary: "plum";
  --space-small: "1rem";
}
```

<!--

Finally, since the design system I work on supports theming. Not just a handful of themes, but dozens or hundreds of themes, we'll also look for a solution that uses CSS variables so we can swap out our theme at runtime to make sure we're not sending a bunch of extra CSS down to the browser.

Note that the examples we look at today will be in React, but the same concepts apply regardless of whatever component-driven JS library you use.
-->

