<template>
  <BaseSection />
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import BaseSection from '@/components/BaseSection.vue'

export default defineComponent({
  components: { BaseSection },
  setup() {
    return {}
  }
})
</script>
<style>
/* アイコンを右端に配置するスタイル */
.v-list-item-action {
  justify-content: flex-end;
}
</style>
