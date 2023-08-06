<template>
  <v-list>
    <v-list-item
      class="mb-6"
      border
      height="60"
      lines="two"
      rounded
      v-for="(item, index) in filteredTodoList"
      :key="index"
    >
      <v-list-item-title>
        {{ item.title }}
      </v-list-item-title>
      <template v-slot:append>
        <v-list-item-action>
          <v-icon @click="handleDelete(item.id)">mdi-delete</v-icon>
        </v-list-item-action>
      </template>
    </v-list-item>
  </v-list>
</template>

<script lang="ts">
import { defineComponent, type PropType } from 'vue'

interface Todo {
  id: Number
  title: string
}

export default defineComponent({
  props: {
    id: {
      type: Number
    },

    filteredTodoList: {
      type: Array as PropType<Todo[]>,
      required: true
    }
  },
  setup(_, context) {
    const handleDelete = (id: Number) => {
      context.emit('handleDelete', id)
    }
    return {
      handleDelete
    }
  }
})
</script>

<style scoped></style>
