
### Drawbacks

 - Atomic classes purging
 - Dynamic inline style assignments

<!-- The drawbacks to all this are, unlike tailwind and purgeCSS, today vanilla-extract does not offer any way to remove the atomic CSS classes that aren't used for a given page.  -->

---

<img src="/assets/ve-purge-css.png" />

<!-- 
https://github.com/vanilla-extract-css/vanilla-extract/pull/942
 -->

 <!-- There is a very interesting PR against vanilla-extract that enables doing this for their Vite plugin, hopefully this can be adopted soon and rolled out to all their plugins.

 For now, the solution is to be discerning about what properties, values, and conditions you use to generate Atomic CSS classes with createSprinkles.
 -->

---

#### Atomic classes purging

 - Limit sprinkles to commonly used values
 - Lean on `style()` for idiosyncratic values/properties
 - Use multiple `defineProperties` to limit conditions


 <!-- Instead of Tailwind where you have a class for basically everything you can possibly ever need. I try to keep sprinkles to commonly used values.
 
 Component-specific styles from .css.ts files that are imported into your components are still tree-shaken when a given component is not used on the page.

 I use multiple defineProperties functions to prevent creating atomic classes at every breakpoint for properties that don't change across conditions -->

---

### Drawbacks

 - Atomic classes purging
 - Dynamic inline style assignments

<!-- Vanilla extract sprinkles doesn't provide a way to do dynamic inline style assignments. So if you're familiar with Tailwind, you know it has this special syntax that allows you to supply any value to properties that might not be thoroughly tokenized in their atomic css system, like width or height -->

---

<img src="/assets/ve-rainbow-sprinkles.png" />

<!-- https://github.com/wayfair/rainbow-sprinkles -->

<!-- There is however, this very cool project by wayfair that basically provides a way to do just that. So hopefully we can look for them to contribute this back to vanilla-extract going forward -->