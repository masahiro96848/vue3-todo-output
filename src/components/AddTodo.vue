<template>
  <v-text-field
    type="text"
    :value="title"
    @input="onInput"
    label="Todoを追加"
    class="mb-4"
    variant="outlined"
    @keypress.enter="handleSubmit"
  ></v-text-field>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    title: {
      type: String,
      default: ''
    }
  },
  setup(_, context) {
    const onInput = (e: Event) => {
      const target = e.target as HTMLInputElement
      context.emit('update:title', target.value)
    }
    const handleSubmit = () => {
      context.emit('handleSubmit')
    }
    return {
      onInput,
      handleSubmit
    }
  }
})
</script>

<style scoped></style>
