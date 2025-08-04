<script setup lang="ts">
defineProps<{
  countsPhone: number[]
  currentCount: number
}>()

defineEmits(['update-count']);
</script>

<template>
  <div class="smartphones-menu__header">
    <h1 class="smartphones-menu__title gray">Смартфоны</h1>
    <div class="smartphones-menu__count-phones blue d-flex">
      <span>Отобразить товары:</span>
      <div class="smartphones-menu__buttons">
        <button
            v-for="count in countsPhone"
            :key="count"
            @click="$emit('update-count', count)"
            class="smartphones-menu__button-count blue"
            :class="{'active': count === currentCount}"
        >
          {{ count }}
        </button>
      </div>
    </div>
  </div>
</template>

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