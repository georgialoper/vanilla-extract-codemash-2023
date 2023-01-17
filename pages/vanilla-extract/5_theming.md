
<img src="/assets/ve-createglobaltheme.png" />

<!-- That's all great, but I'm still hard coding my color values and my space values. I don't like that, let's ameliorate that.

Vanilla extract has several API and some additional libraries that help deal with tokens and theming.

We're not going to cover all of these today, but let's first look at the createGlobalTheme function. -->


---

```ts {1|2|4|5-8|9-13}
// theme.css.ts
import { createGlobalTheme } from "@vanilla-extract/css";

export const vars = createGlobalTheme(":root", {
  color: {
    primary: "navy",
    secondary: "plum",
  },
  space: {
    s: ".5rem",
    m: "1rem",
    l: "2rem",
  },
});
```

<!-- let's take a look at what this looks like. Here I've created a theme.css.ts class, so this will run at build time.

click

I'll import the createGlobalTheme function

click

then I'll export a const called vars, which is sort of an arbitrary name, but one that has taken hold in the vanilla-extract community as short for an object that maps to a set of CSS-variables, which is what this createGlobalTheme function will return.

First param will be an element to define those CSS variables on, it's common to use the root element.

click 

then we'll define our vars object structure. This is also relatively arbitrary, but for organization, we'll define a color key with an object containing all the tokens we want to use as keys and the value those tokens should map to.

click

we'll do the same for our spacing tokens and values -->

---

```css {all|2-3|4-6}
:root {
  --color-primary__asdfg0: "navy";
  --color-secondary__asdfg1: "plum";
  --space-s__asdfg2: "0.5rem";
  --space-m__asdfg3: "1rem";
  --space-l__asdfg4: "2rem";
}
```

<!-- Here is the generated CSS. You can see it's a bunch of CSS variables on the root element. These variables are all hashed and in dev builds the name of the tokens will follow the structure of the keys in the object we supplied.

click

you can see we have our 2 CSS variables for each of our navy and plum color

click

and a CSS variable for each of our 3 spacing values.
 -->

---

```ts
// Heading.css.ts
import { style, styleVariants } from '@vanilla-extract/css'

const space = style({
  padding: "1rem",
});

export const variants = styleVariants({
  primary: [
    space,
    {
      background: "navy",
      color: "plum",
    },
  ],
  secondary: [
    space,
    {
      background: "plum",
      color: "navy",
    },
  ],
})
```

<!-- now we can go back to our Heading.css.ts file -->

---

```ts {3|6|13-14,20-21}
// Heading.css.ts
import { style, styleVariants } from '@vanilla-extract/css'
import { vars } from '../../styles/theme.css'

const space = style({
  padding: vars.space.s,
});

export const variants = styleVariants({
  primary: [
    space,
    {
      background: vars.color.primary,
      color: vars.color.secondary,
    },
  ],
  secondary: [
    space,
    {
      background: vars.color.secondary,
      color: vars.color.primary,
    },
  ],
})
```

<!-- import our vars from our theme

click

replace our hardcoded 1rem value with our space key off the vars object

click 

do the same with our color tokens -->

--- 

```css
.Heading_variants_primary__asdfg1 {
    background: var(--color-primary__asdfg0);
    color: var(--color-secondary__asdfg1);
}

.Heading_space__asdfg0 {
    padding: var(--space-s__asdfg2);
}
```

<!-- When we look at the browser we'll see that our styleVariants classes now reference our CSS variable to supply the color and spacing values -->

---

```ts
// theme.css.ts
import { createGlobalTheme } from "@vanilla-extract/css";

export const vars = createGlobalTheme(":root", {
  color: {
    primary: "navy",
    secondary: "plum",
  },
  space: {
    s: ".5rem",
    m: "1rem",
    l: "2rem",
  },
});
```

<!-- like I said this is just one function vanilla-extract has for abstracting themes -->

---

<img src="/assets/ve-createtheme.png" />

<!--
They also have create theme, which uses the vars structure you supply to define a theme contract all the other themes have to adhere to, enforced by TS

It also returns a generated root class for each theme, so you can swap between themes by swapping that class 
-->

---

<img src="/assets/ve-dynamic.png" />

<!--
There's also this dynamic library that allows you to swap values assigned to the "vars" CSS variables at runtime
-->

---

```tsx {1-2|3|7-17|all}
// app.tsx
import { assignInlineVars } from '@vanilla-extract/dynamic'
import { vars } from '@styles/theme.css.ts'

export const App = () => (
  <div
    style={assignInlineVars(vars, {
      color: {
        primary: "aqua",
        secondary: "teal",
      },
      space: {
        s: "1rem",
        m: "3rem"
        l: "5rem"
      }
    )}>
    { children }
  </div>
)

```

<!--
Basically this allows you to import assignInline vars from vanilla-extract dynamic package

click

import your vars from your theme file

click

then use assignInlineVars to overwrite your vars values. TS will ensure you follow the same object structure

click

super useful if you have to support dozens or hundreds of these where it's impractical to define all the themes at build time and ship them to the browser.

Also useful for creating "preview" interfaces where components immediately change their appearance based on values input in the client, such as a theming admin dashboard. -->