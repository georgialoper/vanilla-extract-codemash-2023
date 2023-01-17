<img src="/assets/ve-style.png"/>

<!--
Vanilla-extract is a library that exports a number of functions that offer different functionality. The most basic of which is the style function.

Let's take a look at how to use this
-->

---

#### VE style function

```ts {all|1|2|4|5-7|all}
// ./components/Heading.css.ts
import { style } from '@vanilla-extract/css'

export const root = style({
  background: "navy",
  color: "plum",
  padding: "1rem"
})
```

<!-- 
First, we'll create a  Heading.css.ts file that lives next to our Heading component.

click

The .css.ts extension is really important in vanilla-extract. It is what tells vanilla extract build plugin to build these typescript files into static CSS which will be sent as CSS to the browser.

So, anything that's in a .css.ts file will happen at build time.
Anything that's in .ts or .tsx will happen at runtime.

click

import style function

click

export a const which we'll call root which will be assigned to the result of that style function

click

to that style function we'll supply an object representing the styles we want to apply.

This will create the same styles we've been looking at, but obviously authored in TS
-->

---

<img src="/assets/ve-ts-info.png"/>

<!--
This is in a .ts file so we'll get Typescript feedback.
If we hover we'll see information about that property since VE is built on top of a CSS types library.
-->

---

<img src="/assets/ve-ts-error.png"/>

---

```tsx
// ./components/Heading.tsx
import React from 'react'

export const Heading = ({ children }) => (
  <div>
    <h1>{ children }</h1>
  </div>
)
```

<!-- Let's go over to our heading component. -->

---

```tsx {3|6|all}
// ./components/Heading.tsx
import React from 'react'
import styles from './Heading.css'

export const Heading = ({ children }) => (
  <div className={ styles.root }>
    <h1>{ children }</h1>
  </div>
)
```

<!--

import styles as a module from .Heading.css. Not heading .css.ts 
There is no CSS file on disk.

We'll take the root key off the styles module and supply that to the className of our element.

Notice this looks exactly like CSS modules. The creators are the same.
-->

---

```tsx {3|6}
// ./components/Heading.tsx
import React from 'react'
import { root } from './Heading.css'

export const Heading = ({ children }) => (
  <div className={ root }>
    <h1>{ children }</h1>
  </div>
)
```

<!-- Just like CSS-modules , we can alternatively destructure our root class in our import 

click 

then pass it directly to our className instead of keying off our styles module -->

---

<img src="/assets/header-component-example-base.png"/>

<!--

And there we have our heading component.

Now let's look at the CSS that's sent down to the browser for this.

-->

---
layout: two-cols
class: 'p-4'
---

#### Dev

```css
.Heading_root_asdfg0 {
  background: "navy";
  color: "plum";
  padding: "1rem";
}
```

::right::

#### Prod

```css
._asdfg0 {
  background: "navy";
  color: "plum";
  padding: "1rem";
}
```

<!--

Remember this is build time CSS in JS so this is from an actual static style sheet.

Our dev build looks exactly like CSS modules.

OUr prod build is just the hash. Note that the last value will just increment for each class defined in the file, so we can put this through gzip or something and get our document down pretty small.
-->

---

```ts
// ./components/Heading.css.ts
import { style } from '@vanilla-extract/css'

export const root = style({
  background: "navy",
  color: "plum",
  padding: "1rem"
})
```
<!-- 
I do want to call out that this does start to differ from CSS modules when you start to do more complicated things with CSS. -->

---

```ts {8-12|all}
// ./components/Heading.css.ts
import { style } from '@vanilla-extract/css'

export const root = style({
  background: "navy",
  color: "plum",
  padding: "1rem"
  selectors: {
    "&:hover": {
      background: "indigo"
    }
  }
})
```

<!--
If you want to target any more complex selectors or pseudo selectors, you need use this kind of selectors object.

Note that, To improve maintainability, each style block can only target a single element. To enforce this, all selectors must start with the & character which is a reference to the current element.

Selectors can also reference other scoped class names, but selectors attempting to target an element other than the current class are invalid.

click

So, vanilla-extract can be pretty opinionated, but overall I find that it helps me author maintainable CSS without personally needing the experience or the foresight to make that judgement myself.
-->