<img src="/assets/ve-globalstyle.png" />

<!-- 
Fortunately, vanilla-extract doesn't completely leave you hanging. If you really need to do something, you can use this globalStyle function.

This creates styles attached to a global selector.
 -->

---

```js
// global.css.ts
import { globalStyle } from '@vanilla-extract/css'

globalStyle('html, body', {
  margin: 0
})
```

<!-- To use it you just import the globalStyle function from vanilla-extract, supply the selector you want to target at the first param, and then your representative style object -->

---

```css
html, body {
  margin: 0;
}
```

<!-- So the output of that would be this CSS, this is super great for your global css reset -->

---

```js {all|2|4|6-8|all}
// global.css.ts
import { globalStyle, style } from '@vanilla-extract/css'

export const rootClass = style({})

globalStyle(`${rootClass} > a`, {
  color: "navy"
})
```

<!-- 
This is a little more complex use case.

click

We also import the style function in addition to the globalStyle function

click

It just takes some representative CSS object and returns a class, so we can supply no CSS and still get ourselves a class.

click

We can then use that generated class in our global style selector
-->

---

```css
.app_rootClass__asdfg0 > a {
  color: "navy";
}
```

<!-- 
So the output of that globalStyle would be this CSS that applies a navy color to any link that is a child of our rootClass -->

---

```tsx {all|2|5|all}
// app.tsx
import { rootClass } from '@styles/global.css'

export const App = () => (
  <div className={rootClass}>
    ...
  </div>
)
```

<!--
The I can go over to my app.tsx component

click

I can import that root class into my component

click

I can then apply it to some element

click

And all links that are a child of that element will be navy. Since this is the root of my app, all links everywhere will be navy.

You can follow this pattern in any component to globally target child nodes just within that component
 -->