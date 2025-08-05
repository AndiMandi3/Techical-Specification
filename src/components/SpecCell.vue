<script setup lang="ts">
import { getCheckIcon } from "../utils/phoneUtils.ts";

defineProps<{
  value: unknown
  specKey: string
  isBoolean: boolean
}>()
</script>

<template>
  <div class="specifications-table__cell">
    <div class="specifications-table__cell-wrapper">
      <template v-if="isBoolean">
        <img
            :src="getCheckIcon(value as boolean)"
            :alt="value as boolean ? 'Да' : 'Нет'"
        />
      </template>
      <template v-else>
        {{ value }}
      </template>
    </div>
  </div>
</template>



<style scoped lang="scss">
.specifications-table {
  &__cell {
    background: #f0f6ff;
    border: 1px solid #e0e0e0;
    border-radius: 6px;
    padding: 12px;
    text-align: center;
    font-size: 14px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  &__cell-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
  }
}

</style>