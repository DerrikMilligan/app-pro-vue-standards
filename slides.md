---
title: Vue Standards
highlighter: shiki
transition: slide-left
---

# Vue Standards

We need em...

---

# Where do we begin?

<v-clicks>

- Eslint... You need it on!
- Align your items with `eslint` so we don't get mixed indentation...
- This can auto-fix 90% of the coding standards, it's very flexible
- Discuss `Interface` at the end of every interface name?
  - Perhaps if it's necessary an `I` prefix, otherwise do away with it

- Stop reaching out to one of Kevin, Derrik, or Dan ("trinity"), instead ask questions in `Vue Questions` so everyone can learn and be edified.
- And for that matter, when someone posts some learning material there, or in `All Programmers` at least have the decency to thumbs up... Even if you don't want to read it at least pretend you care about me...

</v-clicks>

---

# Use Typescript

<v-clicks>

- Nothing should really be untyped or `any`
- If you don't feel comfortable with Typescript, learn it, or ask questions, types save lives
- Try using PHPStan/PHPDoc types in PHP to help you learn. They're similar
- While they can get complicated, 95% of the time they don't need to be
- Make a types folder/file to put your types when you need to actually define an `interface`
  - Keeps your `vue` files cleaner looking and easier to read
  - Same goes for default data.
- You don't have to have an interface for your props, if it's relatively small like ~3-5 items, but if it gets more then move it to a type file

</v-clicks>

---

# Props Example

````md magic-move
```ts
const props = withDefaults(defineProps<{
	actions          ?: Array<ApActionInterface>;
	allowSorting     ?: boolean;
	draggingItems     : boolean;
	items             : Array<ApKanbanItemInterface>;
	sortableGroup    ?: string;
	uncategorizedColor: BaseWaveColor;
	uncategorizedLabel: string;
}>(), {
	actions           : undefined,
	allowSorting      : false,
	bodyBgColor       : 'grey-light6',
	categories        : () => [],
	categoryBgColor   : 'primary',
	draggingItems     : false,
	items             : () => [],
	label             : '',
	sortableGroup     : 'kanban',
	titleBgColor      : 'grey-light6',
	uncategorizedColor: apKanbanPropDefaults.uncategorizedColor,
	uncategorizedLabel: apKanbanPropDefaults.uncategorizedLabel,
});
```
```ts
import { apKanbanColumnPropDefaults, type ApKanbanColumnPropsInterface } from './types/ApKanbanTypes';

const props = withDefaults(defineProps<ApKanbanColumnPropsInterface>(), makePropDefaults(apKanbanColumnPropDefaults));
```
````

---

# Vue File Format

<v-clicks>

- The ordering is `<script>`, `<template>`, `<style>`
- If you break your component out into smaller components, or types, make a folder with the same name as your component, and then nest everything inside it

```
- Job_Ad_Ai_Pro/
 - (components/tabs/category)/
  - CompanyInfoTab.vue
  - JobListingTab.vue
 - types/
   - CompanyInfoTabTypes.ts
   - JobListingTabTypes.ts
   - JobAdAiProTypes.ts
 - JobAdAiPro.vue
```

</v-clicks>

---

# Script

<v-clicks>

- `<script>` should always have `setup` and `lang="ts"`
  - Imports
  - `define` methods `(defineProps, defineEmits, defineExpose)`
  - `ref`, `computed`, other variables
  - methods
- You don't always need `withDefaults`, you can have optional and required props without having to have defaults for everything. And when you do use it, not everything has to have a default value. Only the required fields need defaults if you're going down that rabbit hole

</v-clicks>

---
class: 'text-[15px]'
---

# Template

<v-clicks>

- Don't use bootstrap, pure, or legacy classes! Wave has lots of utility classes and we're adding some more of our own in SCSS, if something is missing we can add more with approval by the trinity. Show `utility.scss`.
  - If something doesn't match the legacy look by default we can adjust Wave, or use the utility classes to compensate
  - Instead of pure use: https://antoniandre.github.io/wave-ui/layout--grid-system
  - Instead of margin/padding use: https://antoniandre.github.io/wave-ui/layout--spaces
    - Watch out for negative margins
- Components can be larger than you think. Not everything has to have it's own component. Unless you're encapsulating:
  - Re-usable templating
  - Business logic that makes sense to extract into it's own component
  - A clean breakpoint, such as a tab in a `w-tabs`
- Use WaveUi components and our shared components.
  - If you think you have a component that we're missing that could be a shared component. Reach out to the trinity and we'll see if we can add it
- `v-for` always has to have a `key` property. Derrik can expound on it

</v-clicks>

---
class: 'text-[14px]'
---

# Style

<v-clicks>

- `<style>` should always have `scoped` and `lang="scss"`
- NEVER use `!important`, learn about CSS specificity instead: https://specificity.keegan.st/
- Don't hard code hex codes. Use either the Domain Admin CSS variables or Wave UI colors. This is also Lee approved and while some color things are still in progress, this is what we need to be striving for
- Try to avoid using `:deep`, but we get that you need to at times
  - Lots of wave things have props to let you pass down additional classes where needed
  - You can often times target the Wave component classes with just normal nested scss, like `.w-card`
  - If we're doing something over and over, like the table header colors we should probably move that to our Wave ui overrides
  - ```css
    :deep(.w-table__header) {
        background-color: var(--ap-table-header-background-color);
        color: var(--ap-table-header-text-color);
    }
    ```
- If we need some global style adjustments, or wave adjustments we have a global SCSS file. These need to be approved by one of the holy trinity
- Do NOT copy the styles from Figma. 90% of those styles, font, line-height and stuff should already be part of our global styles... Just be intentional and know what you're doing. We expect to have competent developers, just take the style tweaks that you need
- Lee is working on all his new designs being straight from Wave and our added components

</v-clicks>

---

# You don't really need a store

For real...

<v-clicks>

- Using `props` and `emits` gets you 99% of the way there
- Will solve strange race conditions if we avoid using them
- Makes us think more about where data needs to actually be instead of being `lazy` and having access to everything in a store
- If you really need it, `provide` and `inject` are available and are still preferable to a store

</v-clicks>

---

# Only use a ref when it makes sense

<v-clicks>

- You don't need `ref` for everything
- There are many cases where just using `props` makes sense
  - They are reactive too!
- You probably never need `reactive`

</v-clicks>

---

# Match the data on the PHP side

<v-clicks>

- The organization of data in a Vue component should start in PHP
- Prep ONLY the data you need
- Allow for an `update` endpoint that expects the same data structure

</v-clicks>

---

# Use Computed

- `computed` is awesome... Use it to mutate your data in a semi-cached way

---

# ApIcon

<v-clicks>

- You should be using ApIcon for pretty much any icon you need to display.
- Don't use the mdi classes, even though we still have to have them for now.
- Pull icons from here: https://icon-sets.iconify.design/mdi/
- Lee is on board with this, however not every design he has currently reflects this yet. Feel free to reach out to him if you can't match an icon and want him to help you pick a new MDI icon

</v-clicks>

---


