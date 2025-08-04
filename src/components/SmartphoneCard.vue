<script setup lang="ts">
import { ref, computed, watch } from "vue"
import type { phoneType } from "../types/phoneType.ts"

const model = defineModel<phoneType>()

const props = defineProps<{
  showSwitcher: boolean
  remainingPhones: phoneType[]
}>()

const windowIsShowed = ref<boolean>(false)
const search = ref<string>('')

const filteredPhones = computed(() =>
    props.remainingPhones.filter(p =>
        p.name.toLowerCase().includes(search.value.toLowerCase())
    )
)

function replacePhone(newPhone: phoneType) {
  model.value = newPhone
  windowIsShowed.value = false
  search.value = ''
}

watch(() => model.value.id, () => {
  windowIsShowed.value = false
  search.value = ''
})

function showReplaceWindow() {
  windowIsShowed.value = !windowIsShowed.value
}
</script>

<template>
  <div class="specifications-table__cell is-header">
    <div class="specifications-table__smartphone-item">
      <div class="specifications-table__smartphone">
        <img :src="model.img" :alt="model.name" />
        <p>{{ model.name }}</p>
      </div>
      <div
          v-if="showSwitcher"
          @click="showReplaceWindow"
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
      <div
          v-if="windowIsShowed"
          class="smartphone-dropdown"
      >
        <input
            v-model="search"
            type="text"
            class="smartphone-dropdown__search"
            placeholder="Поиск телефона..."
        />
        <div v-if="filteredPhones.length > 0" class="smartphone-dropdown__list">
          <div
              v-for="p in filteredPhones"
              :key="p.id"
              class="smartphone-dropdown__item"
          >
            <button @click="replacePhone(p)" class="smartphone-dropdown__replace-btn" title="Заменить">
              <svg width="20" height="20" viewBox="0 0 20 20" fill="none">
                <path d="M19.0909 4.54548H3.10384L6.09735 1.55191C6.45238 1.19694 6.45238 0.621302 6.09735 0.266272C5.74239 -0.0887575 5.16675 -0.0887575 4.81172 0.266272L0.266272 4.81172C-0.0887575 5.16669 -0.0887575 5.74233 0.266272 6.09735L4.81172 10.6428C4.98923 10.8203 5.2219 10.9091 5.45457 10.9091C5.68723 10.9091 5.9199 10.8203 6.09735 10.6428C6.45238 10.2878 6.45238 9.7122 6.09735 9.35717L3.10384 6.36366H19.0909C19.593 6.36366 20 5.95663 20 5.45457C20 4.95251 19.593 4.54548 19.0909 4.54548Z" fill="#36935B"/>
                <path d="M15.1883 9.35717C14.8333 9.0022 14.2576 9.0022 13.9026 9.35717C13.5476 9.71214 13.5476 10.2878 13.9026 10.6428L16.8962 13.6364H0.90912C0.40706 13.6364 3.02863e-05 14.0434 3.02863e-05 14.5455C3.02863e-05 15.0475 0.40706 15.4546 0.90912 15.4546H16.8962L13.9027 18.4481C13.5476 18.8031 13.5476 19.3787 13.9027 19.7338C14.0801 19.9112 14.3128 20 14.5455 20C14.7781 20 15.0108 19.9112 15.1883 19.7337L19.7337 15.1883C20.0887 14.8333 20.0887 14.2576 19.7337 13.9026L15.1883 9.35717Z" fill="#36935B"/>
              </svg>
            </button>
            <img :src="p.img" :alt="p.name" />
            <span>{{ p.name }}</span>
          </div>
        </div>
        <div v-else class="smartphone-dropdown__not-found">
          Телефон не найден
        </div>
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

  &__window-replace {
    padding: 35px 20px;
  }
}

.smartphone-dropdown {
  position: absolute;
  top: 50%;
  right: 17%;
  background: white;
  border: 1px solid #ccc;
  box-shadow: 0 4px 24px 0 #0001;
  padding: 8px;
  z-index: 100;
  min-width: 250px;

  &__search {
    width: 100%;
    margin-bottom: 8px;
    padding: 6px;
    font-size: 14px;
    border: 1px solid #d7d7d7;
    border-radius: 4px;
    outline: none;
  }

  &__item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 20px 0;

    img {
      width: 32px;
      height: auto;
      margin-left: 15px;
    }

    span {
      flex: 1;
      font-size: 14px;
      margin-right: 8px;
      text-align: left;
    }

    .smartphone-dropdown__replace-btn {
      background: none;
      border: none;
      cursor: pointer;
      padding: 0;
    }
  }

  &__not-found {
    text-align: center;
    color: #999;
    font-size: 14px;
    padding: 16px 0 0 0;
  }
}
</style>