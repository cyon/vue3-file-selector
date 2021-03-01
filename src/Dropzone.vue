<template>
  <div
    @dragover="handleDragover"
    @drop.stop.prevent="handleDrop"
    @dragleave="handleDragLeave"
  >
    <slot :hovered="hovered" />
  </div>
</template>
<script>
import { inject, ref } from 'vue'
export default {
  setup () {
    const hovered = ref(false)
    const addFiles = inject('addFiles')

    const handleDrop = (evt) => {
      hovered.value = false
      const files = Array.from(evt.dataTransfer.items).map((item) => {
        if (item.kind !== 'file') return null
        return item.getAsFile()
      }).filter(Boolean)

      addFiles(files)
    }

    const handleDragover = (evt) => {
      evt.preventDefault()
      hovered.value = true
    }

    const handleDragLeave = () => {
      hovered.value = false
    }

    return {
      hovered,
      handleDragover,
      handleDragLeave,
      handleDrop
    }
  }
}
</script>
