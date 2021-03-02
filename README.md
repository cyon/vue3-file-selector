# vue3-file-selector

A Vue 3 headless File Selector component.

## How to use

This library includes a few headless components for a Drag'n Drop supported
file selector.

Here's the parts:

- `FileSelector`: The main container, needs to be used
- `Dropzone`: Handles the Drag'n Drop logic, does not need to be used
- `DialogButton`: Unstyled button that opens the file dialog on click, does not need to be used

## Basic example

```vue
<template>
  <file-selector v-model="files">
    <dropzone v-slot="{ hovered }">
      <div
        class="w-full h-full rounded border-2 border-dashed border-gray-200 transition-colors duration-150 justify-center items-center"
        :class="{ 'border-blue-200': hovered }"
      >
        <ul>
          <li v-for="file in files" :key="file.name">{{ file.name }}</li>
        </ul>
        <dialog-button class="bg-indigo-400 text-white px-2 py-1">Add files...</dialog-button>
      </div>
    </dropzone>
  </file-selector>
</template>
<script>
import { ref } from 'vue'
import { FileSelector, Dropzone, DialogButton } from 'vue3-file-selector'

export default {
  components: {
    FileSelector,
    Dropzone,
    DialogButton
  },
  setup () {
    const files = ref([])

    return {
      files
    }
  }
}
</script>
```

## BYOB (Bring Your Own Button)

This library provides an unstyled button component, which already implements the logic of opening
the file selector dialog. However, if you already have a button component in your project, it would
probably make more sense to use this one.

This can be done as follows:

```vue
<template>
  <file-selector v-model="files" v-slow="{ openDialog }">
  <x-button @click="openDialog">Add files...</x-button>
</template>
```

## The `File` interface

The `v-model` of `FileSelector` is an Array of [`File`](https://developer.mozilla.org/en-US/docs/Web/API/File)
objects as they are returned from a file `<input>` element or the Drag'n Drop `DataTransfer` object.

Here's how you could create a little preview of uploaded images:

```vue
<template>
  <file-selector v-model="files" accept="image/png, image/jpeg">
    <dialog-button>Add images...</dialog-button>
    <div>
      <img v-for="file in files" :key="file.name" :src="toBlob(file)">
    </div>
  </file-selector>
</template>
<script>
import { ref } from 'vue'
import { FileSelector, DialogButton } from 'vue3-file-selector'

export default {
  components: {
    FileSelector,
    DialogButton
  },
  setup () {
    const files = ref([])

    const toBlob = async (file) => {
      const buffer = await file.arrayBuffer()
      const blob = new Blob([buffer])
      const srcBlob = URL.createObjectURL(blob)

      return srcBlob
    }

    return {
      files,
      toBlob
    }
  }
}
</script>
```
