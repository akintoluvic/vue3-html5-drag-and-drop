# Vue 3 Html 5 Drag and Drop

## Introduction

Drag and drop is a common feature in many applications. It allows users to move items around by dragging them from one place and dropping them in another. In this tutorial, we will be building a drag and drop app with Vue 3 and the HTML5 drag and drop API. We will be using the [Vite](https://vitejs.dev/) build tool to scaffold our project. We will also be using the [Tailwind CSS](https://tailwindcss.com/) utility-first CSS framework to style our app. You can find the complete code for this project [here]().

## Project setup

```bash
npm create vue@latest
```

Press enter to proceed, enter a project name, and say no to other options except for Typescript, Eslint and Prettier.

Run the following commands to install dependencies, format code and run the app:

```bash
  cd project-name
  npm install
  npm run format
  npm run dev
```

## Update `tsconfig`, so we can write javascript
To ensure everyone can follow along, we will disable typescript in this project. We will be adding typescript and typing our code at the end of the project. Edit your `tsconfig.app.json` file, under the `compilerOptions`, and add `"allowJs": true` to enable us to write javascript without types. Your `tsconfig.app.json` should look like this:

```json
{
  "extends": "@vue/tsconfig/tsconfig.dom.json",
  "include": ["env.d.ts", "src/**/*", "src/**/*.vue"],
  "exclude": ["src/**/__tests__/*"],
  "compilerOptions": {
    "composite": true,
    "allowJs": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

## Let Vue know we are here for Drag and Drop

Edit the `msg` props value passed to `HelloWorld` component in `App.vue` file to `Drag and Drop` and save the file. The app should reload and display the new value. 

```html
<HelloWorld msg="Drag and Drop" />
```

Then edit the `HelloWorld.vue` file and change the `h3` tag contents to this:

```html
<h3>
  An Html5 Drag and Drop API project with
  <a href="https://vitejs.dev/" target="_blank" rel="noopener">Vite</a> +
  <a href="https://vuejs.org/" target="_blank" rel="noopener">Vue 3</a>.
</h3>
```

## Add Data to render in the app

Our drag and drop functionality will be implemented in `TheWelcome.vue` component. So, let's add some data to render in the app. Edit the `TheWelcome.vue` file, import `ref` from `vue` and add the data we will use to render our list in the `script` tag:

```js
import { ref } from 'vue'

const items = ref([
  {
    icon: DocumentationIcon,
    heading: 'Documentation',
    content:
      'Vue’s official documentation provides you with all information you need to get started.'
  },
  {
    icon: ToolingIcon,
    heading: 'Tooling',
    content:
      'This project is served and bundled with Vite. The recommended IDE setup is VSCode + Volar. '
  },
  {
    icon: EcosystemIcon,
    heading: 'Ecosystem',
    content:
      'Get official tools and libraries for your project: Pinia, Vue Router, Vue Test Utils, and Vue Dev Tools.'
  },
  {
    icon: CommunityIcon,
    heading: 'Community',
    content:
      'Got stuck? You should also subscribe to our mailing list and follow the official @vuejs twitter account for latest news in the Vue world.'
  },
  {
    icon: SupportIcon,
    heading: 'Support Vue',
    content:
      'As an independent project, Vue relies on community backing for its sustainability. You can help us by becoming a sponsor.'
  }
])
```

To prevent typescript errors, remove the `lang="ts"` attribute from the `script` tag.

## Render the data in the app

The vue `template` of `TheWelcome.vue` component currently contains a list of `WelcomeItem` components. We will replace this with a list of `WelcomeItem` components we will be rendering using the data we added in the previous step. Replace the contents of the `template` tag with this:

```html
<WelcomeItem v-for="(item, index) in items" :key="index">
  <template #icon>
    <component :is="item.icon" />
  </template>
  <template #heading>{{ item.heading }}</template>

  {{ item.content }}
</WelcomeItem>
```

If you check your browser console, you will notice that the app is throwing an error, '[Vue warn]: Vue received a Component which was made a reactive object. This can lead to unnecessary performance overhead, and should be avoided by marking the component with `markRaw` or using `shallowRef` instead of `ref`'. Replace `ref` with `shallowRef` to fix this error.

## Update the style of the `WelcomeItem` component

The `WelcomeItem` component is currently using the `WelcomeItem.vue` file. We will update the style of this component to make it look like a card. Edit the `WelcomeItem.vue` file and replace the media query styles in the `style` tag with this:

```css
.dragover, .item:hover {
  outline: 2px solid var(--color-border);
  background-color: var(--color-background-mute);
  border-radius: 15px;
  cursor: pointer;
}

@media (min-width: 1024px) {
  .item {
    margin-top: 0;
    border-radius: 0.5rem;
    padding: 0.4rem 1rem 1rem calc(var(--section-gap) / 2);
  }
  
  i {
    top: 15px;
    left: 20px;
    position: absolute;
    border: 1px solid var(--color-border);
    background: var(--color-background);
    border-radius: 8px;
    width: 50px;
    height: 50px;
  }
}
```

## Add the drag and drop attributes

We will use the `draggable` attribute to make the `WelcomeItem` component draggable. Edit the `TheWelcome.vue` file and add the `draggable` attribute to the `WelcomeItem` tag like this:

```html
<WelcomeItem
    v-for="(item, index) in items"
    :key="index"
    draggable="true"
  >
    <template #icon>
      <component :is="item.icon" />
    </template>
    <template #heading>{{ item.heading }}</template>

    {{ item.content }}
  </WelcomeItem>
  ```

  You can now drag the `WelcomeItem` components around. However, we need to add the drop functionality to make the drag and drop work. Edit the `TheWelcome.vue` file and add the `dragstart`, `dragover` and `drop` event handlers to the `WelcomeItem`. The `dragstart` event fires when an item gets dragged while the `dragover` with `.prevent` ensures that we can drop the dragged item. The `drop` event fires when an item is dropped on another item. Add the event handlers like this:

  ```html
<WelcomeItem
    v-for="(item, index) in items"
    :key="index"
    draggable="true"
    @dragstart="handleDragStart(index)"
    @dragover.prevent
    @drop="handleDrop(index)"
  >
    <template #icon>
      <component :is="item.icon" />
    </template>
    <template #heading>{{ item.heading }}</template>

    {{ item.content }}
  </WelcomeItem>
  ```

## Add the drag and drop methods

First, we will create a variable to store the item being dragged. Edit the `TheWelcome.vue` file and add the `draggedItem` variable to the `script` tag like this:

```js
const draggedItem = ref(0)
```

We will then add the `handleDragStart` and `handleDrop` methods to the `TheWelcome.vue` component. The `handleDragStart` method will be called when an item is dragged. It will set the `draggedItem` to the item being dragged. The `handleDrop` method will be called when an item is dropped. It will remove the `draggedItem` from the list of items and insert it at the index of the item it was dropped on. Edit the `TheWelcome.vue` file and add the methods like this:

```js
const handleDragStart = (startIndex) => {
  draggedItem.value = startIndex
}

const handleDrop = (dropIndex) => {
  let tempArr = [...items.value]
  tempArr.splice(draggedItem.value, 1)
  tempArr.splice(dropIndex, 0, items.value[draggedItem.value])
  items.value = tempArr
}
```

## Conclusion

We have successfully implemented drag and drop functionality in our app. You can now drag and drop the `WelcomeItem` components around. You can find the complete code for this project [here](). To learn more about the HTML5 drag and drop API, you can check out the [MDN docs](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API).

## Next Steps



  ### Issues
  - `Issues with typescript importing from vue`, you can disable typescript in the tsconfig.json file by setting the "noImplicitAny" property to false. This will allow you to import from vue without typescript errors.