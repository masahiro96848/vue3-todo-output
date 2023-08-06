<template>
  <v-dialog v-model="showDeleteDialog">
    <v-card>
      <v-card-title class="headline">Todoを削除</v-card-title>
      <v-card-text>
        以下のTodoを削除しますか？
        <v-list>
          <v-list-item v-for="todo in selectedTodos" :key="todo.id">
            {{ todo.title }}
          </v-list-item>
        </v-list>
      </v-card-text>
      <v-card-actions>
        <v-btn @click="cancel">キャンセル</v-btn>
        <v-btn color="primary" @click="confirmDeleteTodo">削除</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script lang="ts">
import type { PropType } from 'vue'
import { defineComponent, ref, toRefs, watch } from 'vue'

interface Todo {
  id: number
  title: string
}

export default defineComponent({
  name: 'ConfirmDeleteTodo',
  props: {
    // showDeleteDialog: {
    //   type: Boolean,
    //   default: false
    // },
    modelValue: {
      type: Boolean
      // required: true
    },
    selectedTodos: {
      type: Array as PropType<Todo[]>
    }
  },
  setup(props, context) {
    const { modelValue } = toRefs(props)

    const showDeleteDialog = ref(false)
    // 選択されたTodoを格納するためのリアクティブ変数
    const selectedTodos = ref<Todo[]>([])

    // watch(modelValue, (newValue) => {
    //   showDeleteDialog.value = newValue
    // })

    const cancel = () => {
      context.emit('update:modelValue', false)
    }

    const confirmDeleteTodo = () => {
      context.emit('update:modelValue', false)
      context.emit('confirmDeleteTodo')
    }
    return {
      showDeleteDialog,
      // selectedTodos,
      cancel,
      confirmDeleteTodo
    }
  }
})
</script>

<style scoped></style>
