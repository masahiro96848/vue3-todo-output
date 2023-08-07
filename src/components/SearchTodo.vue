<template>
  <v-text-field
    :value="searchKeyword"
    @input="onInput"
    label="キーワード検索"
    variant="outlined"
  ></v-text-field>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  props: {
    searchKeyword: {
      type: String,
      default: ''
    }
  },
  setup(_, context) {
    const onInput = (e: Event) => {
      const target = e.target as HTMLInputElement
      context.emit('update:searchKeyword', target.value)
    }

    return {
      onInput
    }
  }
})
</script>

<style scoped></style>
