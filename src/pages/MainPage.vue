<template>
  <Header />
  <div class="smartphones-menu d-flex">
    <div class="smartphones-menu__header">

      <h1 class="smartphones-menu__title gray">Смартфоны</h1>
      <div class="smartphones-menu__count-phones blue d-flex">
        <span>Отобразить товары:</span>
        <div class="smartphones-menu__buttons">
            <button
                class="smartphones-menu__button-count blue"
                :class="{'active': phone.id === itemOfPhone}"
                v-for="phone in phones.slice(1)"
                :key="phone.id"
                @click="setItemsOfPhone(phone.id)"
            >
              {{phone.id}}
            </button>
        </div>
      </div>
    </div>
    <div class="smartphones-menu__main d-flex">
      <div class="smartphones-menu__differents-checkbox">
        <input type="checkbox" name="smartphones-menu__differents-mobiles">
        <label for="smartphones-menu__differents-mobiles" class="blue">Показать различия</label>
      </div>
      <div class="smartphones-menu__smartphones">
        <div
            class="smartphones-menu__smartphone-item"
            v-for="phoneModel in visiblePhones"
            :key="phoneModel.name">
          <div class="smartphones-menu__smartphone-container">
            <img :src="phoneModel.img" :alt="phoneModel.name">
            <p>{{phoneModel.name}}</p>
          </div>
          <div class="smartphones-menu__switcher-button">
            <svg width="22" height="11" viewBox="0 0 22 11" fill="none" xmlns="http://www.w3.org/2000/svg">
              <path d="M20.375 1.125L11 9.5625L1.625 1.125" stroke="#0D5ADC" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import Header from "../components/Header.vue";

import {onMounted, ref, computed} from "vue";
import type {phoneType} from "../types/phoneType.ts";

const phones = ref<phoneType[]>([])
const itemOfPhone = ref<number>(3)

const visiblePhones = computed(() => {
    return phones.value.slice(0, itemOfPhone.value);
})

function setItemsOfPhone(count: number): void {
  itemOfPhone.value = count;
}

console.log(visiblePhones.value)

async function loadPhones(): Promise<void> {
  try {
    const url = import.meta.env.VITE_URL_API
    const json = await fetch(url);
    phones.value = await json.json();
  }
   catch {
    phones.value = []
   }
}

onMounted(loadPhones);

</script>