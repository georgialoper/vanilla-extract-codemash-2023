---
layout: two-cols
---

<v-clicks>

 - Performance ✅
 - DevX ✅
 - Scoped styles ✅
 - Typesafe ✅
 - Runtime theming ✅
 - Atomic styles ❓

</v-clicks>

::right::

# 🤔

<!-- What has vanilla-extract netted us so far? -->

---

<img src="/assets/ve-sprinkles.png" />

<!-- Vanilla-extract has a seperate library called sprinkles -->

---

```ts {all|2|5|6-10|11|3|12-16|19|6-16}
// sprinkles.css.ts
import { defineProperties, createSprinkles } from "@vanilla-extract/sprinkles";
import { vars } from "./theme.css";

const properties = defineProperties({
  conditions: {
    mobile: {},
    tablet: { "@media": "screen and (min-width: 768px)" },
    desktop: { "@media": "screen and (min-width: 1024px)" },
  },
  defaultCondition: "mobile",
  properties: {
    color: vars.color,
    background: vars.color,
    padding: vars.space,
  }
});

export const sprinklesFn = createSprinkles(properties);
```

<!-- 
So we'll create a file called sprinkles.css.ts, again arbitrary naming since sprinkles is what vanilla-extract has named it's approach to atomic css, personally I might also use atoms.css.ts

import defineProperties and createSprinkles

define const called properties assigned to return of the defineProperties function

conditions can be anything we want here I've defined media query breakpoint conditions for each mobile, tablet and desktop

or in the case of mobile I will define no media query and assign it to our default condition

I might also use these conditions to define contrast mode queries for light and dark mode

then I'll import my vars

use the vars in this properties object where we'll assign our vars to every CSS property for which we'd like to generate an atomic CSS class.

then we'll create a sprinklesFn assigned to the createSprinkles function which accepts our properties

Which is a little convoluted all we really need to focus on in here is defining our properties
 -->

---

```css {all|1-6|7-12|13-21|all}
.sprinkles_color_primary_mobile__asdfg0 {
  color: var(--color-primary__asdfg0)
}
.sprinkles_color_secondary_mobile__asdfg0 {
  color: var(--color-secondary__asdfg1)
}
.sprinkles_background_primary_mobile__asdfg0 {
  background: var(--color-primary__asdfg2)
}
.sprinkles_background_secondary_mobile__asdfg0 {
  background: var(--color-secondary__asdfg3)
}
.sprinkles_space_s_mobile__asdfg0 {
  padding: var(--space-s__asdfg4)
}
.sprinkles_space_m_mobile__asdfg0 {
  padding: var(--space-m__asdfg4)
}
.sprinkles_space_m_mobile__asdfg0 {
  padding: var(--space-m__asdfg4)
}
```

<!-- 
This is the CSS output, again it's a css.ts file so this will be static CSS created at build time.
You can see we have a class for every vars token for every property we defined in that properties key, so we effectively have Atomic CSS classes

We have our color atomic classes

background color atomic classes

space atomic classes

they're all getting their values from our CSS variables

notice that these class names follow the pattern of filename, property name, token name, then we have our default condition name.
 -->

---

```css
@media screen and (min-width: 768px) {
  .sprinkles_color_primary_tablet__asdfg0 {
  color: var(--color-primary__asdfg0)
}
.sprinkles_color_secondary_tablet__asdfg0 {
  color: var(--color-secondary__asdfg1)
}
.sprinkles_background_primary_tablet__asdfg0 {
  background: var(--color-primary__asdfg2)
}
.sprinkles_background_secondary_tablet__asdfg0 {
  background: var(--color-secondary__asdfg3)
}
.sprinkles_space_s_tablet__asdfg0 {
  padding: var(--space-s__asdfg4)
}
.sprinkles_space_m_tablet__asdfg0 {
  padding: var(--space-m__asdfg4)
}
.sprinkles_space_m_tablet__asdfg0 {
  padding: var(--space-m__asdfg4)
}
}
```

<!-- So you can infer that we're also getting a set of atomic classes scoped to the tablet breakpoint -->

---

```css {all|15,18,21}
@media screen and (min-width: 1024px) {
.sprinkles_color_primary_desktop__asdfg0 {
  color: var(--color-primary__asdfg0)
}
.sprinkles_color_secondary_desktop__asdfg0 {
  color: var(--color-secondary__asdfg1)
}
.sprinkles_background_primary_desktop__asdfg0 {
  background: var(--color-primary__asdfg2)
}
.sprinkles_background_secondary_desktop__asdfg0 {
  background: var(--color-secondary__asdfg3)
}
.sprinkles_space_s_desktop__asdfg0 {
  padding: var(--space-s__asdfg4)
}
.sprinkles_space_m_desktop__asdfg0 {
  padding: var(--space-m__asdfg4)
}
.sprinkles_space_m_desktop__asdfg0 {
  padding: var(--space-m__asdfg4)
}
}
```

<!--

And a set of classes scoped to the desktop breakpoint

click

One thing that's not very atomic in that these spacing atomic classes are just targetting the padding property, so all sides of the element will receive padding,

I think we want finer grained control over that
-->

---

```ts {7-13|14-18|15|16-17}
// sprinkles.css.ts
import { defineProperties, createSprinkles } from "@vanilla-extract/sprinkles";
import { vars } from "./theme.css";

const properties = defineProperties({
  // ...
  properties: {
    // ...
    paddingTop: vars.space,
    paddingBottom: vars.space,
    paddingLeft: vars.space,
    paddingRight: vars.space,
  }
  shorthands: {
    padding: ["paddingTop", "paddingBottom", "paddingLeft", "paddingRight"],
    paddingX: ["paddingLeft", "paddingRight"],
    paddingY: ["paddingTop", "paddingBottom"],
  },
});

export const sprinklesFn = createSprinkles(properties);
```

<!-- so let's actually go back to our properties and instead of just supplying our space to padding, generally, we'll supply space to each side individually.

Then we can use this shorthands key to create shorthands that will compose together the classes for the properties we define in the corresponding array.

Padding 

padding X & Y
-->

---

```ts
// Heading.css.ts
import { style } from '@vanilla-extract/css'
import { vars } from '../../styles/theme.css'

export const root = style({
  background: vars.color.primary,
  color: vars.color.secondary,
  padding: vars.space.s,
})
```

<!-- let's go back to the basic version of our heading.css.ts file using the styles function.

Instead of using our vars... -->

---

```ts {3|5,11|6-10}
// ./components/Heading.css.ts
import { style } from '@vanilla-extract/css'
import { sprinklesFn } from '../../styles/sprinkles.css'

export const root = style([
  sprinklesFn({
    background: "primary",
    color: "secondary",
    padding: "s"
  })
])
```

<!--
we can import our sprinklesfn

instead of passing some representative CSS object to our style functionwe'll supply an array to the style function. 

Inside that array we'll supply an object to our sprinkles function that contains each property we want to style, but instead of specifying a hardcoded value or our vars to that property, we can pass a key from the object in our vars that we specified for that property in our properties key.

So, when we defined our properties we said background and color can take vars.color, so those properties can accept our primary or secondary keys
-->

---

```css {all|1-6|7-20}
.sprinkles_background_primary_mobile__asdfg6 {
    background: var(--color-primary__asdfg1);
}
.sprinkles_color_secondary_mobile__asdfg3 {
    color: var(--color-secondary__asdfg2);
}
.sprinkles_paddingRight_s_mobile__asdfg13 {
    padding-right: var(--space-s__asdfg3);
}
.sprinkles_paddingLeft_s_mobile__asdfgu {
    padding-left: var(--space-s__asdfg3);
}
.sprinkles_paddingBottom_s_mobile__asdfgl {
    padding-bottom: var(--space-s__asdfg3);
}
.sprinkles_paddingTop_s_mobile__asdfgc {
    padding-top: var(--space-s__asdfg3);
}
```

<!-- This would be the atomic CSS classes applied to our component -->

---

```ts {9}
// ./components/Heading.css.ts
import { style } from '@vanilla-extract/css'
import { sprinklesFn } from '../../styles/sprinkles.css'

export const variants = style([
  sprinklesFn({
    background: "primary",
    color: "secondary",
    paddingX: "s"
  })
])
```

<!-- we can instead use our paddingX shorthand  -->

---

```css {6-14}
.sprinkles_background_primary_mobile__asdfg6 {
    background: var(--color-primary__asdfg1);
}
.sprinkles_color_secondary_mobile__asdfg3 {
    color: var(--color-secondary__asdfg2);
}
.sprinkles_paddingRight_s_mobile__asdfg13 {
    padding-right: var(--space-s__asdfg3);
}
.sprinkles_paddingLeft_s_mobile__asdfgu {
    padding-left: var(--space-s__asdfg3);
}
```

<!-- this will  just supply the padding left and padding right atomic CSS classes  -->

---

```ts {9-13}
// ./components/Heading.css.ts
import { style } from '@vanilla-extract/css'
import { sprinklesFn } from '../../styles/sprinkles.css'

export const variants = style([
  sprinklesFn({
    background: "primary",
    color: "secondary",
    paddingLeft: {
      mobile: "s",
      tablet: "m",
      desktop: "l"
    }
  })
])
```

<!-- we can supply what we'll call a media query object to, say, paddingLeft, where we can supply a different vars space key or token for each condition we defined -->

---

```css {all|7-9|10-14|15-19|all}
.sprinkles_background_primary_mobile__asdfg6 {
  background: var(--color-primary__asdfg1);
}
.sprinkles_color_secondary_mobile__asdfg3 {
  color: var(--color-secondary__asdfg2);
}
.sprinkles_paddingLeft_s_mobile__asdfgu {
  padding-left: var(--space-s__asdfg3);
}
@media screen and (min-width: 768px) {
  .sprinkles_paddingLeft_m_tablet__asdfgu {
    padding-left: var(--space-m__asdfg3);
  }
}
@media screen and (min-width: 1024px) {
  .sprinkles_paddingLeft_l_desktop__asdfgu {
    padding-left: var(--space-l__asdfg3);
  }
}
```

<!--
here's those classes 

mobile

tablet

desktop

Imporant: all the atomic classes for each condition will exist regardless of if we use them in a component's .css.ts file or not.
-->

---

```tsx {all|1|3|7-11}
// ./components/Heading.tsx
import React from 'react'
import { sprinklesFn } from '../../styles/sprinkles.css'

export const Heading = ({ children }) => (
  <div
    className={sprinklesFn({
      background: "primary",
      color: "secondary",
      padding: "s"
    })}
  >
    <h1>{ children }</h1>
  </div>
)
```
<!-- 
so instead of defining our heading styles in our .css.ts file at build time, we can actually go and call our sprinkles function in our heading component to compose our styles at no runtime cost.

Since the atomic classes already exist and are there in the browser, only the lockup needs to happen at runtime.

so we can import our sprinkles fn

and supply the result of that function directly to the className prop -->

---

```ts {1|18-20|20}
// ./styles/sprinkles.css.ts
import { defineProperties, createSprinkles } from "@vanilla-extract/sprinkles";
import { vars } from "./theme.css";

const properties = defineProperties({
  conditions: {
    ...
  },
  defaultCondition: "mobile",
  properties: {
    ...
  },
  shorthands: {
    ...
  },
});

export const sprinklesFn = createSprinkles(properties);

export type Sprinkles = Parameters<typeof sprinklesFn>[0];
```

<!-- if we go back to our sprinkles.css.ts file where we defined our sprinkles Fn

we can use our sprinklesFn to create a Sprinkles type

representing the parameters that can be supplied to that function -->

---

```tsx {1|3|5-9|11|13-17|all}
// ./components/Heading.tsx
import React from 'react'
import { sprinklesFn, Sprinkles } from '../../styles/sprinkles.css'

type HeadingProps = {
  background?: Sprinkles["background"];
  color?: Sprinkes["color"]
  padding?: Sprinkes["padding"]
}

export const Heading = ({ background, color, padding, children }: HeadingProps) => (
  <div
    className={sprinklesFn({
      background,
      color,
      padding
    })}
  >
    <h1>{ children }</h1>
  </div>
)
```

<!-- 

then we can go back to our heading component

import that Sprinkles type

use it to type all the properties we want our parent component to be able to style via props, being background, color, and padding,

use that heading props type to and surface those props on our component

then we can pass those props right on through to the object we're supplying our sprinkles fn

em

So now we have this component. that has strongly typed all it's props that can be passed from a parent component. which will stay in sync with the styles we're creating atomic classes for in our sprinkles. and these classes will be reused by any component that uses this sprinkles function

One thing that I'm not crazy about is now our component code is pretty tightly coupled to our styles implementation. What can be done about that?
-->
