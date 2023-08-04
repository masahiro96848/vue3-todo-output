<template>
  <v-app>
    <v-main>
      <v-container class="mt-12">
        <v-row justify="center">
          <v-col cols="12" sm="8" md="6">
            <h1 class="text-center">Todo List</h1>
            <v-card class="pa-10">
              <v-form>
                <v-text-field variant="outlined"></v-text-field>
                <v-list>
                  <v-list-item
                    class="mb-6"
                    border
                    height="60"
                    lines="two"
                    rounded
                    v-for="todo in todos"
                    :key="todo"
                  >
                    {{ todo }}
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
import { defineComponent, ref } from 'vue'
export default defineComponent({
  setup() {
    const todos = ref(['テスト1', 'テスト2', 'テスト3'])

    return {
      todos
    }
  }
})
</script>
