<template>
  <input style="display: none;" type="file" ref="inputRef" :multiple="allowMultiple" :accept="accept" @change="updateFiles">
  <slot :openDialog="openDialog" />
</template>
<script>
import { provide, ref } from 'vue'
export default {
  emits: ['update:modelValue'],
  props: {
    modelValue: {
      type: Array,
      required: true
    },
    allowMultiple: {
      type: Boolean,
      default: true
    },
    accept: {
      type: Array,
      default: undefined
    }
  },
  setup (props, { emit }) {
    const inputRef = ref(null)
    const openDialog = () => {
      inputRef.value.click()
    }

    provide('openDialog', openDialog)

    provide('addFiles', (files) => {
      emit('update:modelValue', [...props.modelValue, ...files])
    })

    const updateFiles = () => {
      emit('update:modelValue', [...props.modelValue, ...inputRef.value.files])
    }

    return {
      inputRef,
      openDialog,
      updateFiles
    }
  }
}
</script>
