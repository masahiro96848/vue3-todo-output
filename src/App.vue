<template>
  <v-app>
    <v-main>
      <v-container class="mt-12">
        <v-row justify="center">
          <v-col cols="12" sm="8" md="6">
            <h1 class="text-center">Todo List</h1>
            <v-card class="pa-10">
              <v-form>
                <v-text-field
                  v-model="title"
                  label="Todoを追加"
                  class="mb-4"
                  variant="outlined"
                  @keyup.enter="handleSubmit"
                ></v-text-field>
                <v-text-field
                  v-model="searchKeyword"
                  label="キーワード検索"
                  variant="outlined"
                ></v-text-field>

                <!-- TodoList一覧 -->
                <!-- <v-list>
                  <v-list-item
                    class="mb-6"
                    border
                    height="60"
                    lines="two"
                    rounded
                    v-for="item in filteredTodoList"
                    :key="item.id"
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
                </v-list> -->
                <TodoList :filtered-todo-list="filteredTodoList" @handleDelete="handleDelete" />
              </v-form>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>

    <!-- ダイアログ -->
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
          <v-btn @click="showDeleteDialog = false">キャンセル</v-btn>
          <v-btn color="primary" @click="confirmDeleteTodo">削除</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script lang="ts">
import { defineComponent, ref, computed } from 'vue'
import AddTodo from './components/AddTodo.vue'
import TodoList from './components/TodoList.vue'
import ConfirmDeleteTodo from '@/components/modal/ConfirmDeleteTodo.vue'

interface Todo {
  id: number
  title: string
}

export default defineComponent({
  components: { AddTodo, TodoList },
  setup() {
    const title = ref('')
    const searchKeyword = ref('')
    const nextTodoId = ref(4)
    const showDeleteDialog = ref(false)
    // 選択されたTodoを格納するためのリアクティブ変数
    const selectedTodos = ref<Todo[]>([])

    let todoList = ref<Todo[]>([
      { id: 1, title: 'テスト1' },
      { id: 2, title: 'テスト2' },
      { id: 3, title: 'テスト3' }
    ])

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

    const handleDelete = (id: number) => {
      const selectedTodo = todoList.value.find((item) => item.id === id)
      if (selectedTodo) {
        selectedTodos.value = [selectedTodo]
        showDeleteDialog.value = true
      }
    }

    const confirmDeleteTodo = async () => {
      const selectedIds = selectedTodos.value.map((todo) => todo.id)
      todoList.value = todoList.value.filter((todo) => !selectedIds.includes(todo.id))
      selectedTodos.value = [] // 選択された Todo をリセット

      showDeleteDialog.value = false
      console.log(todoList)
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
