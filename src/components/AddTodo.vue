<template>
  <v-text-field
    :value="title"
    type="text"
    @input="onInput"
    label="Todoを追加"
    class="mb-4"
    variant="outlined"
    @keyup.enter="handleSubmit"
  ></v-text-field>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  props: {},
  setup(_, context) {
    const title = ref('')
    const onInput = (e: Event) => {
      const target = e.target as HTMLInputElement
      context.emit('update:modelValue', target.value)
    }
    const handleSubmit = () => {
      // context.emit('handleSubmit')
    }
    return {
      title,
      onInput,
      handleSubmit
    }
  }
})
</script>

<style scoped></style>
