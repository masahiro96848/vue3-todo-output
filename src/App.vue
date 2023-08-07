<template>
  <v-app>
    <v-main>
      <v-container class="mt-12">
        <v-row justify="center">
          <v-col cols="12" sm="8" md="6">
            <h1 class="text-center">Todo List</h1>
            <v-card class="pa-10">
              <v-form>
                <!-- Todo新規追加 -->
                <AddTodo v-model:title="title" @handleSubmit="handleSubmit" />
                <!-- Todoキーワード検索 -->
                <SearchTodo v-model:searchKeyword="searchKeyword" />
                <!-- TodoList一覧 -->
                <TodoList :filtered-todo-list="filteredTodoList" @handleDelete="handleDelete" />
              </v-form>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue'
import AddTodo from './components/AddTodo.vue'
import SearchTodo from './components/SearchTodo.vue'
import TodoList from './components/TodoList.vue'
import { INIT_TODO_LIST } from '@/constants/todo'

interface Todo {
  id: number
  title: string
}

export default defineComponent({
  components: { AddTodo, SearchTodo, TodoList },
  setup() {
    const title = ref('')
    const searchKeyword = ref('')
    const nextTodoId = ref(4)
    const showDeleteDialog = ref(false)
    const selectedTodos = ref<Todo[]>([]) // 選択されたTodoを格納するためのリアクティブ変数

    const todoList = ref<Todo[]>(INIT_TODO_LIST)

    const filteredTodoList = computed(() => {
      const keyword = searchKeyword.value.trim().toLowerCase()
      if (!keyword) {
        return todoList.value
      } else {
        return todoList.value.filter((item) => item.title.toLowerCase().startsWith(keyword))
      }
    })

    const handleSubmit = () => {
      if (title.value === '') return
      const newTodo: Todo = {
        id: nextTodoId.value,
        title: title.value.trim() // 入力値をトリムしてから使用
      }
      todoList.value.push(newTodo) // 新しいTodoをtodosに追加
      title.value = '' // 入力フィールドをクリア

      nextTodoId.value++ // 次のTodoのIDを更新
    }

    const handleDelete = (id: number, targetTitle: string) => {
      if (window.confirm(`「${targetTitle}」のtodoを削除しますか？`)) {
        todoList.value = todoList.value.filter((todo) => todo.id !== id)
      }
    }

    const cancel = () => {
      showDeleteDialog.value = false
    }

    const confirmDeleteTodo = () => {
      const selectedIds = selectedTodos.value.map((todo) => todo.id)
      todoList.value = todoList.value.filter((todo) => !selectedIds.includes(todo.id))
      selectedTodos.value = [] // 選択された Todo をリセット

      showDeleteDialog.value = false
    }

    return {
      title,
      searchKeyword,
      todoList,
      showDeleteDialog,
      selectedTodos,
      filteredTodoList,
      handleSubmit,
      handleDelete,
      cancel,
      confirmDeleteTodo
    }
  }
})
</script>
<style>
/* アイコンを右端に配置するスタイル */
.v-list-item-action {
  justify-content: flex-end;
}
</style>
