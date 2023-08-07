<template>
  <BaseSection />
</template>

<script lang="ts">
import { defineComponent, ref, watch, reactive, computed, provide, toRefs } from 'vue'
import BaseSection from '@/components/BaseSection.vue'
import { INIT_TODO_LIST } from '@/constants/Todo'
import type { TodoType } from '@/interfaces/Todo'

export default defineComponent({
  components: { BaseSection },
  setup() {
    const searchKeyword = ref('')
    const todoList = ref<TodoType[]>(INIT_TODO_LIST)

    const filteredTodoList = computed(() => {
      const keyword = searchKeyword.value.trim().toLowerCase()
      if (!keyword) {
        return todoList.value
      } else {
        return todoList.value.filter((item) => item.title.toLowerCase().startsWith(keyword))
      }
    })

    // filteredTodoListをProvide
    provide('filteredTodoList', filteredTodoList)

    return {
      searchKeyword,
      todoList,
      filteredTodoList
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
./constants/Todo
