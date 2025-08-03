<script setup lang="ts">
import type {phoneType} from "../types/phoneType.ts";

defineProps<{
  phone: phoneType
  showSwitcher: boolean
}>()
</script>

<template>
  <div class="specifications-table__cell is-header">
    <div class="specifications-table__smartphone-item">
      <div class="specifications-table__smartphone">
        <img :src="phone.img" :alt="phone.name" />
        <p>{{ phone.name }}</p>
      </div>
      <div
          v-if="showSwitcher"
          class="specifications-table__switcher-button"
      >
        <svg width="22" height="11" viewBox="0 0 22 11" fill="none">
          <path
              d="M20.375 1.125L11 9.5625L1.625 1.125"
              stroke="#0D5ADC"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
          />
        </svg>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.specifications-table {

  &.is-header {
    font-weight: 600;
    background: white;
  }

  &__smartphone-item {
    display: flex;
    flex-direction: row;
    align-items: center;
  }

  &__switcher-button {
    margin-top: 6px;
    cursor: pointer;
  }
}
</style>