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
                <v-text-field label="キーワード検索" variant="outlined"></v-text-field>
                <v-list>
                  <v-list-item
                    class="mb-6"
                    border
                    height="60"
                    lines="two"
                    rounded
                    v-for="item in todoList"
                    :key="item.id"
                  >
                    {{ item.title }}
                  </v-list-item>
                </v-list>
              </v-form>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import { defineComponent, ref, reactive } from 'vue'

interface Todo {
  id: number
  title: string
}

export default defineComponent({
  setup() {
    const title = ref('')
    const nextTodoId = ref(4)
    const todoList: Todo[] = reactive([
      { id: 1, title: 'テスト1' },
      { id: 2, title: 'テスト2' },
      { id: 3, title: 'テスト3' }
    ])

    const handleSubmit = () => {
      if (title.value === '') return
      const newTodo: Todo = {
        id: nextTodoId.value,
        title: title.value.trim() // 入力値をトリムしてから使用
      }
      todoList.push(newTodo) // 新しいTodoをtodosに追加
      title.value = '' // 入力フィールドをクリア
      nextTodoId.value++ // 次のTodoのIDを更新
    }

    return {
      title,
      todoList,
      handleSubmit
    }
  }
})
</script>
