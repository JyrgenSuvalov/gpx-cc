---
# try also 'default' to start simple
theme: ./theme
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: './rubin.png'
# some information about your slides (markdown enabled)
title: CC w/ OS 4 GPX
info: |
  ## Intro to using Claude Code with OpenSpec to build software
# apply UnoCSS classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable Comark Syntax: https://comark.dev/syntax/markdown
comark: true
---

# CC w/ OS 4 GPX

<!--
Tentative title
-->

---
transition: slide-left
class: 'flex items-center justify-center !p-0'
---

<img src="/xkcd.jpg" class="max-h-full max-w-full object-contain" />

<!--
Why are we here?
-->

---
transition: slide-left
layout: image-right
image: /ralph.jpg
---

# Goals

<v-clicks>

- Feel comfortable using the CC TUI to build software using the OpenSpec framework
- A better understanding of the current state of the art
- Ideas for further research

</v-clicks>

<v-click>

## Non-goals

- Building a unicorn B2B SaaS startup in a weekend

</v-click>

---
class: text-center
---

# Structure

<div class="flex items-center justify-center gap-10 mt-32 text-6xl">
  <div class="px-10 py-8 bg-blue-500/20 rounded-2xl">Lecture</div>
  <div class="text-5xl">→</div>
  <div class="px-10 py-8 bg-green-500/20 rounded-2xl">Setup</div>
  <div class="text-5xl">→</div>
  <div class="px-10 py-8 bg-orange-500/20 rounded-2xl">Build</div>
</div>

---
layout: image-right
image: https://cover.sli.dev
---

# Code

Use code snippets and get the highlighting directly, and even types hover!

```ts [filename-example.ts] {all|4|6|6-7|9|all} twoslash
// TwoSlash enables TypeScript hover information
// and errors in markdown code blocks
// More at https://shiki.style/packages/twoslash
import { computed, ref } from 'vue'

const count = ref(0)
const doubled = computed(() => count.value * 2)

doubled.value = 2
```

<arrow v-click="[4, 5]" x1="350" y1="310" x2="195" y2="342" color="#953" width="2" arrowSize="1" />

<!-- This allow you to embed external code blocks -->

<<< @/snippets/external.ts#snippet

<!-- Footer -->

[Learn more](https://sli.dev/features/line-highlighting)

<!-- Inline style -->
<style>
.footnotes-sep {
  @apply mt-5 opacity-10;
}
.footnotes {
  @apply text-sm opacity-75;
}
.footnote-backref {
  display: none;
}
</style>

<!--
Notes can also sync with clicks

[click] This will be highlighted after the first click

[click] Highlighted with `count = ref(0)`

[click:3] Last click (skip two clicks)
-->

---
level: 2
---

# Shiki Magic Move

Powered by [shiki-magic-move](https://shiki-magic-move.netlify.app/), Slidev supports animations across multiple code snippets.

Add multiple code blocks and wrap them with <code>````md magic-move</code> (four backticks) to enable the magic move. For example:

````md magic-move {lines: true}
```ts {*|2|*}
// step 1
const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery',
  ],
})
```

```ts {*|1-2|3-4|3-4,8}
// step 2
export default {
  data() {
    return {
      author: {
        name: 'John Doe',
        books: [
          'Vue 2 - Advanced Guide',
          'Vue 3 - Basic Guide',
          'Vue 4 - The Mystery',
        ],
      },
    }
  },
}
```

```ts
// step 3
export default {
  data: () => ({
    author: {
      name: 'John Doe',
      books: [
        'Vue 2 - Advanced Guide',
        'Vue 3 - Basic Guide',
        'Vue 4 - The Mystery',
      ],
    },
  }),
}
```

Non-code blocks are ignored.

```vue
<!-- step 4 -->
<script setup>
const author = {
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery',
  ],
}
</script>
```
````

---

# Components

<div grid="~ cols-2 gap-4">
<div>

You can use Vue components directly inside your slides.

We have provided a few built-in components like `<Tweet/>` and `<Youtube/>` that you can use directly. And adding your custom components is also super easy.

```html
<Counter :count="10" />
```

<!-- ./components/Counter.vue -->
<Counter :count="10" m="t-4" />

Check out [the guides](https://sli.dev/builtin/components.html) for more.

</div>
<div>

```html
<Tweet id="1390115482657726468" />
```

<Tweet id="1390115482657726468" scale="0.65" />

</div>
</div>

<!--
Presenter note with **bold**, *italic*, and ~~striked~~ text.

Also, HTML elements are valid:
<div class="flex w-full">
  <span style="flex-grow: 1;">Left content</span>
  <span>Right content</span>
</div>
-->

---
class: px-20
---

# Themes

Slidev comes with powerful theming support. Themes can provide styles, layouts, components, or even configurations for tools. Switching between themes by just **one edit** in your frontmatter:

<div grid="~ cols-2 gap-2" m="t-2">

```yaml
---
theme: default
---
```

```yaml
---
theme: seriph
---
```

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-default/01.png?raw=true" alt="">

<img border="rounded" src="https://github.com/slidevjs/themes/blob/main/screenshots/theme-seriph/01.png?raw=true" alt="">

</div>

Read more about [How to use a theme](https://sli.dev/guide/theme-addon#use-theme) and
check out the [Awesome Themes Gallery](https://sli.dev/resources/theme-gallery).

---

# Clicks Animations

You can add `v-click` to elements to add a click animation.

<div v-click>

This shows up when you click the slide:

```html
<div v-click>This shows up when you click the slide.</div>
```

</div>

<br>

<v-click>

The <span v-mark.red="3"><code>v-mark</code> directive</span>
also allows you to add
<span v-mark.circle.orange="4">inline marks</span>
, powered by [Rough Notation](https://roughnotation.com/):

```html
<span v-mark.underline.orange>inline markers</span>
```

</v-click>

<div mt-20 v-click>

[Learn more](https://sli.dev/guide/animations#click-animation)

</div>

---

# Motions

Motion animations are powered by [@vueuse/motion](https://motion.vueuse.org/), triggered by `v-motion` directive.

```html
<div
  v-motion
  :initial="{ x: -80 }"
  :enter="{ x: 0 }"
  :click-3="{ x: 80 }"
  :leave="{ x: 1000 }"
>
  Slidev
</div>
```

<div class="w-60 relative">
  <div class="relative w-40 h-40">
    <img
      v-motion
      :initial="{ x: 800, y: -100, scale: 1.5, rotate: -50 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-square.png"
      alt=""
    />
    <img
      v-motion
      :initial="{ y: 500, x: -100, scale: 2 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-circle.png"
      alt=""
    />
    <img
      v-motion
      :initial="{ x: 600, y: 400, scale: 2, rotate: 100 }"
      :enter="final"
      class="absolute inset-0"
      src="https://sli.dev/logo-triangle.png"
      alt=""
    />
  </div>

  <div
    class="text-5xl absolute top-14 left-40 text-[#2B90B6] -z-1"
    v-motion
    :initial="{ x: -80, opacity: 0}"
    :enter="{ x: 0, opacity: 1, transition: { delay: 2000, duration: 1000 } }">
    Slidev
  </div>
</div>

<!-- vue script setup scripts can be directly used in markdown, and will only affects current page -->
<script setup lang="ts">
const final = {
  x: 0,
  y: 0,
  rotate: 0,
  scale: 1,
  transition: {
    type: 'spring',
    damping: 10,
    stiffness: 20,
    mass: 2
  }
}
</script>

<div
  v-motion
  :initial="{ x:35, y: 30, opacity: 0}"
  :enter="{ y: 0, opacity: 1, transition: { delay: 3500 } }">

[Learn more](https://sli.dev/guide/animations.html#motion)

</div>

---

# $\LaTeX$

$\LaTeX$ is supported out-of-box. Powered by [$\KaTeX$](https://katex.org/).

<div h-3 />

Inline $\sqrt{3x-1}+(1+x)^2$

Block

$$
{1|3|all}
\begin{aligned}
\nabla \cdot \vec{E} &= \frac{\rho}{\varepsilon_0} \\
\nabla \cdot \vec{B} &= 0 \\
\nabla \times \vec{E} &= -\frac{\partial\vec{B}}{\partial t} \\
\nabla \times \vec{B} &= \mu_0\vec{J} + \mu_0\varepsilon_0\frac{\partial\vec{E}}{\partial t}
\end{aligned}
$$

[Learn more](https://sli.dev/features/latex)

---

# Diagrams

You can create diagrams / graphs from textual descriptions, directly in your Markdown.

<div class="grid grid-cols-4 gap-5 pt-4 -mb-6">

```mermaid {scale: 0.5, alt: 'A simple sequence diagram'}
sequenceDiagram
    Alice->John: Hello John, how are you?
    Note over Alice,John: A typical interaction
```

```mermaid {theme: 'neutral', scale: 0.8}
graph TD
B[Text] --> C{Decision}
C -->|One| D[Result 1]
C -->|Two| E[Result 2]
```

```mermaid
mindmap
  root((mindmap))
    Origins
      Long history
      ::icon(fa fa-book)
      Popularisation
        British popular psychology author Tony Buzan
    Research
      On effectiveness<br/>and features
      On Automatic creation
        Uses
            Creative techniques
            Strategic planning
            Argument mapping
    Tools
      Pen and paper
      Mermaid
```

```plantuml {scale: 0.7}
@startuml

package "Some Group" {
  HTTP - [First Component]
  [Another Component]
}

node "Other Groups" {
  FTP - [Second Component]
  [First Component] --> FTP
}

cloud {
  [Example 1]
}

database "MySql" {
  folder "This is my folder" {
    [Folder 3]
  }
  frame "Foo" {
    [Frame 4]
  }
}

[Another Component] --> [Example 1]
[Example 1] --> [Folder 3]
[Folder 3] --> [Frame 4]

@enduml
```

</div>

Learn more: [Mermaid Diagrams](https://sli.dev/features/mermaid) and [PlantUML Diagrams](https://sli.dev/features/plantuml)

---
foo: bar
dragPos:
  square: 691,32,167,_,-16
---

# Draggable Elements

Double-click on the draggable elements to edit their positions.

<br>

###### Directive Usage

```md
<img v-drag="'square'" src="https://sli.dev/logo.png">
```

<br>

###### Component Usage

```md
<v-drag text-3xl>
  <div class="i-carbon:arrow-up" />
  Use the `v-drag` component to have a draggable container!
</v-drag>
```

<v-drag pos="663,206,261,_,-15">
  <div text-center text-3xl border border-main rounded>
    Double-click me!
  </div>
</v-drag>

<img v-drag="'square'" src="https://sli.dev/logo.png">

###### Draggable Arrow

```md
<v-drag-arrow two-way />
```

<v-drag-arrow pos="67,452,253,46" two-way op70 />

---
src: ./pages/imported-slides.md
hide: false
---

---

# Monaco Editor

Slidev provides built-in Monaco Editor support.

Add `{monaco}` to the code block to turn it into an editor:

```ts {monaco}
import { ref } from 'vue'
import { emptyArray } from './external'

const arr = ref(emptyArray(10))
```

Use `{monaco-run}` to create an editor that can execute the code directly in the slide:

```ts {monaco-run}
import { version } from 'vue'
import { emptyArray, sayHello } from './external'

sayHello()
console.log(`vue ${version}`)
console.log(
  emptyArray<number>(10).reduce(
    (fib) => [...fib, fib.at(-1)! + fib.at(-2)!],
    [1, 1],
  ),
)
```

---
layout: center
---

# PileUp

Press <kbd>space</kbd> to stack 'em up

<PileUp
  :images="[
    '/Screenshot 2026-04-14 at 21.20.04.png',
    '/Screenshot 2026-04-14 at 21.21.17.png',
    '/Screenshot 2026-04-14 at 21.21.52.png',
    '/Screenshot 2026-04-14 at 21.22.38.png',
    '/Screenshot 2026-04-14 at 21.44.00.png',
    '/Screenshot 2026-04-14 at 22.07.05.png',
  ]"
  width="400px"
  class="mt-8"
/>

---
class: '!p-0 !overflow-hidden relative'
---

<ImageGrid
  :images="[
    '/gridImages/Screenshot 2026-04-14 at 22.25.25.png',
    '/gridImages/Screenshot 2026-04-14 at 22.25.43.png',
    '/gridImages/Screenshot 2026-04-14 at 22.25.50.png',
    '/gridImages/Screenshot 2026-04-14 at 22.28.44.png',
    '/gridImages/Screenshot 2026-04-14 at 22.29.07.png',
    '/gridImages/Screenshot 2026-04-14 at 22.29.23.png',
    '/gridImages/Screenshot 2026-04-14 at 22.29.28.png',
    '/gridImages/Screenshot 2026-04-14 at 22.29.41.png',
    '/gridImages/Screenshot 2026-04-14 at 22.29.59.png',
    '/gridImages/Screenshot 2026-04-14 at 22.30.04.png',
    '/gridImages/Screenshot 2026-04-14 at 22.30.21.png',
    '/gridImages/Screenshot 2026-04-14 at 22.30.24.png',
    '/gridImages/Screenshot 2026-04-14 at 22.30.33.png',
    '/gridImages/Screenshot 2026-04-14 at 22.30.45.png',
    '/gridImages/Screenshot 2026-04-14 at 22.30.55.png',
    '/gridImages/Screenshot 2026-04-14 at 22.31.03.png',
    '/gridImages/Screenshot 2026-04-14 at 22.31.37.png',
    '/gridImages/Screenshot 2026-04-14 at 22.31.45.png',
    '/gridImages/Screenshot 2026-04-14 at 22.31.54.png',
    '/gridImages/Screenshot 2026-04-14 at 22.32.06.png',
    '/gridImages/Screenshot 2026-04-14 at 22.32.27.png',
    '/gridImages/Screenshot 2026-04-14 at 22.32.50.png',
    '/gridImages/Screenshot 2026-04-14 at 22.33.11.png',
    '/gridImages/Screenshot 2026-04-14 at 22.33.18.png',
    '/gridImages/Screenshot 2026-04-14 at 22.33.30.png',
    '/gridImages/Screenshot 2026-04-14 at 22.33.39.png',
    '/gridImages/Screenshot 2026-04-14 at 22.33.49.png',
    '/gridImages/Screenshot 2026-04-14 at 22.34.11.png',
    '/gridImages/Screenshot 2026-04-14 at 22.34.15.png',
    '/gridImages/Screenshot 2026-04-14 at 22.34.27.png',
    '/gridImages/Screenshot 2026-04-14 at 22.34.43.png',
    '/gridImages/Screenshot 2026-04-14 at 22.34.54.png',
    '/gridImages/Screenshot 2026-04-14 at 22.35.29.png',
    '/gridImages/Screenshot 2026-04-14 at 22.35.39.png',
    '/gridImages/Screenshot 2026-04-14 at 22.35.46.png',
    '/gridImages/Screenshot 2026-04-14 at 22.36.17.png',
    '/gridImages/Screenshot 2026-04-14 at 22.36.30.png',
    '/gridImages/Screenshot 2026-04-14 at 22.36.52.png',
  ]"
/>

---
layout: cover
---

<SlidevVideo v-click autoplay class="mx-auto">
  <source src="/switch.mp4" type="video/mp4" />
</SlidevVideo>

---
layout: center
---

<Youtube id="5dEp1ZpYDUg?start=88" width="900" height="506" />

---
layout: center
class: text-center
---

# Learn More

[Documentation](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/resources/showcases)

<PoweredBySlidev mt-10 />
