# vue3-file-selector

A Vue 3 headless File Selector component.

## How to use

This library includes a few headless components for a drag and drop supported
file selector.

Here's the parts:

- `FileSelector`: The main container, needs to be used
- `Dropzone`: Handles the drag and drop logic, does not need to be used
- `DialogButton`: Unstyled button that opens the file dialog on click, does not need to be used

## Basic example

Here's a basic example with drag and drop and a list of the selected files.

```vue
<template>
  <file-selector v-model="files">
    <dropzone v-slot="{ hovered }">
      <div
        class="block w-full h-64 rounded-lg border-4 border-dashed border-gray-400 transition-colors duration-150 flex flex-col space-y-4 justify-center items-center"
        :class="{ 'border-blue-200': hovered }"
      >
        <ul>
          <li v-for="file in files" :key="file.name">
            {{ file.name }}
          </li>
        </ul>
        <dialog-button class="bg-indigo-400 rounded text-white px-2 py-1"
          >Add files...</dialog-button
        >
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
objects as they are returned from a file `<input>` element or the drag and drop `DataTransfer` object.

Here's how you could create a little preview of uploaded images:

```vue
<template>
  <file-selector v-model="files" :accept="['image/png', 'image/jpeg']">
    <dialog-button>Add images...</dialog-button>
    <div>
      <img v-for="preview in previews" :key="preview" :src="preview" />
    </div>
  </file-selector>
</template>
<script>
import { ref, watch } from 'vue'
import { FileSelector, DialogButton } from 'vue3-file-selector'

export default {
  components: {
    FileSelector,
    DialogButton,
  },
  setup() {
    const files = ref([])
    const previews = ref([])

    const toBlob = async (file) => {
      const buffer = await file.arrayBuffer()
      const blob = new Blob([buffer])
      const srcBlob = URL.createObjectURL(blob)

      return srcBlob
    }

    watch(files, async () => {
      previews.value = await Promise.all(
        files.value.map((file) => toBlob(file))
      )
    })

    return {
      files,
      previews,
    }
  },
}
</script>
```
