<template>
  <div class="smartphones-menu__header">
    <h1 class="smartphones-menu__title gray">Смартфоны</h1>
    <div class="smartphones-menu__count-phones blue d-flex">
      <span>Отобразить товары:</span>
      <div class="smartphones-menu__buttons">
        <button
            v-for="phone in slicedPhones"
            :key="phone.id"
            @click="$emit('update-count', phone.id)"
            class="smartphones-menu__button-count blue"
            :class="{'active': phone.id === currentCount}"
        >
          {{ phone.id }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type {phoneType} from "../types/phoneType.ts";

defineProps<{
  slicedPhones: phoneType[]
  currentCount: number
}>()

defineEmits(['update-count']);
</script>

<style scoped lang="scss">
.smartphones-menu {
  &__header {
    display: inherit;
    justify-content: space-between;
    align-items: center;
    width: 100%;
  }
  &__title {
    font-size: 48px;
    font-family: "Roboto Bold", sans-serif;
  }
  &__count-phones {
    gap: 10px;
  }

  &__buttons {
    display: inherit;
    gap: 5px;
  }

  &__buttons button {
    background: transparent;
    border: none;
    cursor: pointer;
    font-family: "Roboto-Regular", sans-serif;
  }
  &__main {
    margin-top: 20px;
  }
}

</style>