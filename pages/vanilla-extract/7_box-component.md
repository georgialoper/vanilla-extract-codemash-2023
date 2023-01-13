
<img src="/assets/box-mui.png" />

<!-- If you familiar with mui, they have this Box component, which is like this wildcard and serves to bind any element you want to their styling system -->

---

<img src="/assets/box-chakra.png" />

<!-- Chakra UI has the same concept, at this point we can pretty simply replicate this -->

---

```tsx
// ./components/Box.tsx
import React from 'react'

type BoxProps = {
  children: React.ReactNode;
};

export const Box = ({ children }: BoxProps) => (
  <div>{ children }</div>
);
```

<!-- here's a basic div component -->

---

```tsx {5|9|10}
// ./components/Box.tsx
import React from 'react'

type BoxProps = {
  as: React.ElementType
  children: React.ReactNode;
};

export const Box = ({ as: As, children }: BoxProps) => (
  <As>{ children }</As>
);
```

<!--
We can expose some prop that can accept any element type(heading tags, p, section, etc.), Chakra calls this prop as

we can cast that prop to PascalCase

Then render that like we would any other custom component

-->

---

```tsx {4|9|12|13}
// ./components/Box.tsx
import React from 'react'

import { sprinklesFn, Sprinkles } from "../../styles/sprinkles.css";

type BoxProps = {
  as: React.ElementType;
  children: React.ReactNode;
  sprinkles: Sprinkles;
};

export const Box = ({ as: As, sprinkles, children }: BoxProps) => (
  <As className={sprinklesFn(sprinkles)}>{children}</As>
);
```
<!-- 
then we'll import our sprinkles function and type

use the type to define a sprinkles prop, which again is an object representing any of the atomic classes we generated

expose sprinkles as a prop on our component

pass the value of that prop through to our sprinklesfn, the output of this function will be supplied to the className of the element -->

---

```tsx
// ./components/Heading.tsx
import React from 'react'
import { sprinklesFn, Sprinkles } from '../../styles/sprinkles.css'

type HeadingProps = {
  background?: Sprinkles["background"];
  color?: Sprinkles["color"]
  padding?: Sprinkles["padding"]
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

<!-- We can then go back to our heading component -->

---

```tsx {3|4|13-15|all}
// ./components/Heading.tsx
import React from 'react'
import { Sprinkles } from '../../styles/sprinkles.css'
import { Box } from './Box'

type HeadingProps = {
  background?: Sprinkles["background"];
  color?: Sprinkles["color"]
  padding?: Sprinkles["padding"]
}

export const Heading = ({ background, color, padding, children }: HeadingProps) => (
  <Box as="div" sprinkles={{ background, color, padding }}>
    <h1>{ children }</h1>
  </Box>
)
```

<!-- remove the import of our sprinkles fn

import our box component

replace our root div with our Box component and supply our background color and padding props to the Box's sprinkles prop as an object

So now we have completely abstracted our style implementation away from our heading component, and we have this utility Box component we can use to bind our styles implementation with all our components.

So, hypothetically, mui and chakra can rip out their runtime CSS-in-JS styles and replace it with vanilla-extract without touching any their components expect for their Box component.
-->

---

<img src="/assets/ve-dessert-box.png" />

<!--
You don't even need to write your own box component, there's this cool dessert-box project that'll do all this for you.
https://github.com/TheMightyPenguin/dessert-box
-->

---
layout: two-cols
---

 - Performance ✅
 - DevX ✅
 - Scoped styles ✅
 - Typesafe ✅
 - Runtime theming ✅
 - Atomic styles ✅

::right::

<img src="/assets/guy-fieri-chefs-kiss.gif" />

<!-- So, now vanilla-extract covers our whole pie in the sky wish list, but it's not quite all roses  -->