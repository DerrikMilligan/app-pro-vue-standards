---
title: Vue Standards
highlighter: shiki
transition: slide-left
---

# Vue Standards

We need em...

---

# Where do we begin?

- Eslint... You need it on!
- Align your items with `eslint` so we don't get mixed indentation...
- This can auto-fix 90% of the coding standards, it's very flexible
- Discuss `Interface` at the end of every interface name?
  - Perhaps if it's necessary an `I` prefix, otherwise do away with it

---

# Use Typescript

- Nothing should really be untyped or `any`
- If you don't feel comfortable with Typescript, learn it
- Try using PHPStan/PHPDoc types in PHP to help you learn. They're similar
- While they can get complicated, 95% of the time they don't need to be
- Make a types folder/file to put your types when you need to actually define an `interface`
  - Keeps your `vue` files cleaner looking and easier to read
  - Same goes for default data.

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

# You don't really need a store

For real...

- Using `props` and `emits` gets you 99% of the way there
- Will solve strange race conditions if we avoid using them
- Makes us think more about where data needs to actually be instead of being `lazy` and having access to everything
- If you really need it, `provide` and `inject` are available and are still preferable to a store

---

# Only use a ref when it makes sense

- You don't need `ref` for everything
- You probably never need `reactive`
- There are many cases where just using `props` makes sense
  - They are technically reactive too!

---

# Match the data on the PHP side

- The organization of data in a Vue component should start in PHP
- Prep ONLY the data you need
- Allow for an `update` endpoint that expects the same data structure

---

# Use Computed

- `computed` is awesome... Use it to mutate your data in a semi-cached way
