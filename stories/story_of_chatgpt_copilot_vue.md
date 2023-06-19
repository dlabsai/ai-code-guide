# Story

I was asked to build a vue3 application with map that shows various points of interest. 
Having a knowledge of JS, Angular and React I didn't touch vue3. With help of chatgpt and copilot I was manage to catch quickly. Bellow I show you an examples of the prompts I was using.

Let's start

# Results

* The app was finished,
* all tasks were completed before end of deadline,
* and most importantly, client was happy with the results (and the pace of development)

# Prompt

Person: 
- using vue3 and vuetify, create a card component with image, title, description and show full picture button

ChatGPT: 
- To create a card component with Vue 3 and Vuetify, you need to make sure that you have installed and configured both Vue 3 and Vuetify in your project. If you haven't done that, you can do it by running the following commands:

```graphql

npm install vue@next
npm install @vue/compiler-sfc
npm install vuetify@next
```



Now let's create a new file named "CardComponent.vue" in your components directory and put the following code into it:

```vue

<template>
  <v-card>
    <v-img
      :src="image"
      aspect-ratio="1.5"
      class="white--text align-end"
    >
      <v-card-title v-text="title" />
    </v-img>

    <v-card-subtitle v-text="description"></v-card-subtitle>

    <v-card-actions>
      <v-btn color="primary" text @click="showFullImage">Show Full Picture</v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
export default {
  props: {
    image: {
      type: String,
      required: true,
    },
    title: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
  },
  methods: {
    showFullImage() {
      window.open(this.image, '_blank');
    },
  },
};
</script>

<style scoped>
</style>
```



This is a basic example of a card component with Vue 3 and Vuetify. It uses props to receive the image, title, and description from its parent component. The `showFullImage` method is used to open the image in a new browser tab when the "Show Full Picture" button is clicked.

You can now use this component in your Vue application like so:

```vue

<template>
  <div>
    <CardComponent
      image="https://example.com/image.jpg"
      title="Card Title"
      description="This is a description"
    />
  </div>
</template>

<script>
import CardComponent from './CardComponent.vue';

export default {
  components: {
    CardComponent,
  },
};
</script>
```



Just replace the `image`, `title`, and `description` attributes with your own values.

Person: 
- use camel case for vuetify component name like 'VCard' instead 'v-card'. Apply this to all generated components

ChatGPT: 
- Sure! Here's how you would convert the kebab-case component names to PascalCase (CamelCase) in the Vue and Vuetify code:

```vue

<template>
  <VCard>
    <VImg
      :src="image"
      aspect-ratio="1.5"
      class="white--text align-end"
    >
      <VCardTitle v-text="title" />
    </VImg>

    <VCardSubtitle v-text="description"></VCardSubtitle>

    <VCardActions>
      <VBtn color="primary" text @click="showFullImage">Show Full Picture</VBtn>
    </VCardActions>
  </VCard>
</template>

<script>
export default {
  props: {
    image: {
      type: String,
      required: true,
    },
    title: {
      type: String,
      required: true,
    },
    description: {
      type: String,
      required: true,
    },
  },
  methods: {
    showFullImage() {
      window.open(this.image, '_blank');
    },
  },
};
</script>

<style scoped>
</style>
```



In the parent component, you would import the components like so:

```vue

<template>
  <div>
    <CardComponent
      image="https://example.com/image.jpg"
      title="Card Title"
      description="This is a description"
    />
  </div>
</template>

<script>
import CardComponent from './CardComponent.vue';

export default {
  components: {
    CardComponent,
  },
};
</script>
```



This code works the same as the previous one, but it uses PascalCase (CamelCase) for component names. As of the writing of this response, both kebab-case and PascalCase are acceptable and commonly used in the Vue.js community.

Note: As of Vue 3, the kebab-case is considered the conventional style for HTML and is still necessary for DOM templates. PascalCase or camelCase is typically used in JSX or when you're using the `h()` function to create elements.

Remember to update your import and usage according to the conventions you've set in your project.

Person: 
- what are the key concepts of vue 3

ChatGPT: 
- Vue 3 introduces several new concepts and updates, while also preserving many of the core ideas from Vue 2. Here are the key concepts introduced with Vue 3: 
1. **Composition API** : The most significant change in Vue 3 is the introduction of the Composition API. This is an addition to the Options API, which was the default way of writing components in Vue 2. The Composition API offers a more flexible way to manage code in larger Vue applications, providing improved reusability, readability, and organization of code. 
2. **Reactivity System** : Vue 3 introduces a new reactivity system under the hood, which is more efficient and consistent. It's based on JavaScript's Proxy object, which allows Vue to track changes to data more accurately and with less overhead than in Vue 2. 
3. **Teleport Component** : Vue 3 introduces the `<teleport>` component (formerly known as `<portal>` in Vue 2), which allows you to define a component that's anchored in one part of the template, but its content can be "teleported" and rendered in a different part of the DOM. This is particularly useful for modals, notifications, pop-ups, etc. 
4. **Fragments** : Unlike Vue 2, which requires each component to have a single root element, Vue 3 supports fragments, which means you can have multiple root nodes in your Vue template. 
5. **Suspense Component** : Suspense is a new feature in Vue 3 that simplifies async operations in your templates. It allows Vue components to "wait" for some asynchronous activity to finish before they render. 
6. **Improved TypeScript Integration** : Vue 3 is written in TypeScript, and as such, it offers much better TypeScript support out of the box. 
7. **Multiple v-models** : In Vue 2, a component could have only one v-model directive, which was typically used for input components to create two-way data bindings. Vue 3 allows for multiple v-model bindings on the same component, which gives you more flexibility when working with complex components. 
8. **Performance Improvements** : Vue 3 includes a variety of optimizations and performance improvements, including smaller bundle sizes, faster rendering, improved memory usage, and more.

These concepts reflect the evolution of Vue.js and demonstrate its flexibility and commitment to providing robust solutions for building single-page applications and complex user interfaces.

Person: 
- write watchable list that will change if the list change

ChatGPT: 
- Here is an example of using Vue 3's Composition API to create a watchable list:

```vue

<template>
  <div>
    <p>List:</p>
    <ul>
      <li v-for="(item, index) in list" :key="index">{{ item }}</li>
    </ul>
    <button @click="addToList">Add item</button>
  </div>
</template>

<script>
import { ref, watchEffect } from 'vue'

export default {
  setup() {
    const list = ref(['item 1', 'item 2', 'item 3'])

    // watchEffect will run whenever any reactive dependencies (like 'list') are changed
    watchEffect(() => {
      console.log('List changed:', list.value)
    })

    function addToList() {
      list.value.push(`item ${list.value.length + 1}`)
    }

    return {
      list,
      addToList
    }
  }
}
</script>
```



In this code, the `list` is a reactive reference to an array. The `watchEffect` function will run its callback every time `list` changes, logging the new list to the console. When the "Add item" button is clicked, a new item is added to the list, triggering the `watchEffect` callback.

This is a simple example, but it can be expanded upon to suit more complex use cases.

Person: 
- write a custom store using composition api

ChatGPT: 
- Creating a store using the Composition API is quite simple and provides a flexible way to manage state across your Vue 3 application. Here's a very basic example:

First, we will create our store in a separate file. You can call it `store.js`:

```javascript

import { reactive } from 'vue'

const state = reactive({
  count: 0
})

const increment = () => {
  state.count++
}

const decrement = () => {
  state.count--
}

export default {
  state,
  increment,
  decrement
}
```



In this code, `state` is a reactive object that will contain our state, and `increment` and `decrement` are functions that mutate this state.

Then, you can use the store in your Vue components like this:

```vue

<template>
  <div>
    <p>Count: {{ state.count }}</p>
    <button @click="increment">Increment</button>
    <button @click="decrement">Decrement</button>
  </div>
</template>

<script>
import store from './store.js'

export default {
  setup() {
    return {
      state: store.state,
      increment: store.increment,
      decrement: store.decrement
    }
  }
}
</script>
```



This is a very basic store that only manages a single count value. If you needed a more complex store with modules, actions, and getters, you might want to consider using Vuex, which is the official state management library for Vue, and it also has support for the Composition API in its latest version. However, for simple use cases or for small applications, this approach with a custom store using the Composition API can work well.
