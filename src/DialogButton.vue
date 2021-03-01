<template>
  <button v-bind="$attrs" @click="openDialog">
    <slot />
  </button>
</template>
<script>
import { inject } from 'vue'
export default {
  setup () {
    const openDialog = inject('openDialog')

    return {
      openDialog
    }
  }
}
</script>
