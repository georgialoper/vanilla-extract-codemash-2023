
<img src="/assets/ve-stylevariants.png"/>

<!-- So, with just those 2 functions, you can do most things you could with css-modules or any other tooling. So now we'll see where vanilla-extract starts to really shine.

Let's look at this styleVariants function, which creates a collection of named style rules.

This is really helpful is you want to change a set of styles based on a single prop.
 -->

---

```ts {1|2|4|5-9|10-14|all}
// ./components/Heading.css.ts
import { styleVariants } from '@vanilla-extract/css'

export const variants = styleVariants({
  primary: {
    background: "navy",
    color: "plum",
    padding: "1rem"
  },
  secondary: {
    background: "plum",
    color: "navy",
    padding: "1rem"
  },
})
```

<!-- Back in my Heading.css.ts file

click

import styleVariants instead of style

click

export const variants assigned to result of styleVariant function

click

supply a couple keys- primary which will be an object representative of our base CSS

click

then I'll supply a secondary key which will supply the inverse background and color values.

click

I can keep going for any number of variants
-->

---

```tsx {3|5|6}
// ./components/Heading.tsx
import React from 'react'
import { variants } from './Heading.css'

export const Heading = ({ variant, children }) => (
  <div className={ variants[variant] }>
    <h1>{ children }</h1>
  </div>
)
```

<!-- I can then import my variants in my heading component

click

expose a variant prop on my component

click

and use that prop to key which of my variants was requested by the parent component -->

---

```tsx {all|6}
// app.tsx
import { Heading } from './components/Heading'

export const App = () => (
  <>
    <Heading variant="primary">🧁 Hello, CodeMash!</Heading>
  </>
);
```

<!-- So if I go into my app component

click

and supply primary to the variant prop I'll get our base set of styles -->

---

```tsx {6}
// app.tsx
import { Heading } from './components/Heading'

export const App = () => (
  <>
    <Heading variant="secondary">🧁 Hello, CodeMash!</Heading>
  </>
);
```

<!-- If I supply secondary to the variant prop... -->

---

<img src="/assets/header-component-example-variant.png" />

<!-- I'll get the inverse styles I supplied my styleVariants secondary object -->

---

```tsx {5-9}
// ./components/Heading.tsx
import React from 'react'
import { variants } from './Heading.css'

type HeadingProps = {
  variant: keyof typeof variants
}

export const Heading = ({ variant, children }: HeadingProps) => (
  <div className={ variants[variant] }>
    <h1>{ children }</h1>
  </div>
)
```

<!-- What's really cool, is that since I defined this all in TS, I can type my heading component's variant prop using keyof typeof variants

assign that type to my heading component props -->

---

<img src="/assets/ve-ts-error-variant.png" />

<!-- Then if I misspell any of the variants or try to supply a variant that doesn't exist, I'll get that TS feedback telling my I can only use primary or secondary.

Likewise, if I were to end up adding a tertiary key to my styleVariants object, the type of this variant prop on my Heading component would stay up to date, without having to go make that update in the Heading

It would just become automatically available on the parent component -->

---

```ts {8,13}
// ./components/Heading.css.ts
import { styleVariants } from '@vanilla-extract/css'

export const variants = styleVariants({
  primary: {
    background: "navy",
    color: "plum",
    padding: "1rem"
  },
  secondary: {
    background: "plum",
    color: "navy",
    padding: "1rem"
  },
})
```

<!--
I'm not a huge fan that we applied the same padding to each variant

We can abstract that to its own class and compose it together in our variants.
-->

---

```ts {2|4-6|9-15|9-22|12-13,19-20}
// ./components/Heading.css.ts
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

<!-- So if we import style from vanilla extract

click

use it to define a space class with our padding

click

then instead of having our primary key being just an object of the styles we want,
it'll be an array that composes together that space class that we're getting from the style function as well as the class from the object represeting the primary styles we want

click

We can do that for our secondary key as well

click

now we see that the only properties in our variants are those that actually contain differing styles
 -->

---

```css
.Heading_variants_primary__asdfg1 {
    background: navy;
    color: plum;
}

.Heading_space__asdfg0 {
    padding: 1rem;
}
```
<!-- 
This is the CSS that will return for the primary variant of my header component. 

you can see I have one class for my background and color properties and another class for my padding

so you can infer that the styleVariants function is actually returning one class for every item in my primary array instead of duplicating the CSS from the space class our style function created. -->