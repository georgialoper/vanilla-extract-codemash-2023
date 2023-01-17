
<img src="/assets/tsocss-intro.png" />

<!--

So what approaches are we going to be evaluating today?

The State of CSS 2022 was released maybe a few weeks or months ago now and they largely break down today's approaches to styling to 2 broad categories.

 - CSS Frameworks
 - CSS-in-JS
-->

---

<img src="/assets/tsocss-css-frameworks.png" />

<!-- There's a lot of CSS frameworks out there, most of them are opinionated about their design but may offer some degree of customization. Bootstrap remains ever present. -->

---

<img src="/assets/tsocss-tailwind.png" />

<!--

but far and away the most popular framework right now is Tailwind.
Now tailwind is really just a flavor of Atomic CSS.

Atomic CSS for the most part is where 1 class applies styles to exactly one CSS property.

Tachyons follows the same concept. If you've every tried to apply margins or paddings in Bootstrap or Bulma you've used atomic CSS to some degree.

You've probably even authored some atomic CSS in your application or used atomic CSS classes authored by someone on your team.

So today we'll take a look at Atomic CSS generally. 
-->

---

<img src="/assets/tsocss-css-in-js.png" />

<!--
CSS-in-JS is the other broad category of approaches to styling modern JavaScript applications.

There's a lot of frameworks and libraries in this category that take very different approaches.
-->

---

<img src="/assets/tsocss-css-modules.png" />

<!--

The most dominant approach to CSS-in-JS is far and away CSS-modules. In fact, it is so popular it ships out of the box with NextJs and I believe Remix just added this as well.

Most of these libraries outside of vanilla-extract and CSS-modules are what we'll call Runtime CSS-in-JS, which is a large umbrella, so let's dig into that first.

-->