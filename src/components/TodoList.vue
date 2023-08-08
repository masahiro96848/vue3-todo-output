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
          <v-icon @click="handleDelete(item.id, item.title)">mdi-delete</v-icon>
        </v-list-item-action>
      </template>
    </v-list-item>
  </v-list>
</template>

<script lang="ts">
import { defineComponent, inject } from 'vue'
import type { TodoType } from '@/interfaces/Todo'

export default defineComponent({
  setup(_, context) {
    // InjectでfilteredTodoListを取得
    const filteredTodoList = inject('filteredTodoList') as TodoType[]
    const handleDelete = (targetId: Number, targetTitle: String) => {
      context.emit('handleDelete', targetId, targetTitle)
    }

    return {
      filteredTodoList,
      handleDelete
    }
  }
})
</script>

<style scoped></style>
