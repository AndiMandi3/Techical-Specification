<template>
  <Header/>
  <!-- SmartPhonesView -->
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
            {{ phone.id }}
          </button>
        </div>
      </div>
    </div>
    <div class="smartphones-menu__main d-flex">
      <table class="specifications-table">
        <thead>
        <tr>
          <th class="specifications-table__checkbox-differents">
            <input
                type="checkbox"
                name="specifications-table__differents-mobiles"
                v-model="showOnlyDiffs"
            >
            <label for="specifications-table__checkbox-differents" class="blue">Показать различия</label>
          </th>
          <th
              v-for="phoneModel in visiblePhones"
              :key="phoneModel.name"
          >
            <div class="specifications-table__smartphone-item">
              <div class="specifications-table__smartphone-container">
                <img :src="phoneModel.img" :alt="phoneModel.name">
                <p>{{ phoneModel.name }}</p>
              </div>
              <template v-if="itemOfPhone < 6">
                <div class="specifications-table__switcher-button">
                  <svg width="22" height="11" viewBox="0 0 22 11" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path d="M20.375 1.125L11 9.5625L1.625 1.125" stroke="#0D5ADC" stroke-width="2" stroke-linecap="round"
                          stroke-linejoin="round"/>
                  </svg>
                </div>
              </template>
            </div>
          </th>
        </tr>
        </thead>
        <tbody>
        <template
            v-for="specKey in specKeys"
            :key="specKey">
          <tr v-if="!showOnlyDiffs || diffsKeys.includes(specKey)">
            <td class="specifications-table__label">{{ specificationLabels[specKey] }}</td>
            <td v-for="phone in visiblePhones" :key="phone.id">
              <template v-if="['nfc', 'esim', 'wirelessCharging'].includes(specKey)">
                <img
                    :src="getCheckIcon(phone[specKey])"
                    :alt="phone[specKey] ? 'Да' : 'Нет'"
                />
              </template>
              <template v-else-if="specKey === 'price'">
                {{ phone[specKey] }} ₽
              </template>
              <template v-else>
                {{ phone[specKey] }}
                <span v-if="specKey === 'screenSize'">″</span>
                <span v-if="specKey === 'fps'">Гц</span>
                <span v-if="specKey === 'memorySize'">ГБ</span>
              </template>
            </td>
          </tr>
        </template>
        </tbody>
      </table>
    </div>
  </div>
</template>


<script setup lang="ts">
  import Header from "../components/Header.vue"

  import {onMounted, ref, computed} from "vue"
  import type {phoneType} from "../types/phoneType.ts"

  const phones = ref<phoneType[]>([])
  const itemOfPhone = ref<number>(3)
  const showOnlyDiffs = ref<boolean>(false)

  const visiblePhones = computed(() => {
    return phones.value.slice(0, itemOfPhone.value)
  })

  function setItemsOfPhone(count: number): void {
    itemOfPhone.value = count
  }

  async function loadPhones(): Promise<void> {
    try {
      const url = import.meta.env.VITE_URL_API
      const json = await fetch(url);
      phones.value = await json.json();
    } catch {
      phones.value = []
    }
  }

  const specificationLabels: Record<string, string> = {
    manufacture: "Производитель",
    release: "Год релиза",
    screenSize: "Диагональ экрана (дюйм)",
    country: "Страна-производитель",
    memorySize: "Объём памяти",
    fps: "Частота обновления экрана",
    nfc: "NFC",
    esim: "Поддержка eSIM",
    wirelessCharging: "Поддержка беспроводной зарядки",
    price: "Стоимость",
  }

  const specKeys = Object.keys(specificationLabels)

  const diffsKeys = computed(() => {
    const result: string[] = []

    for(let key of specKeys) {
      const value = visiblePhones.value.map((phone) => phone[key])
      const isDifferent = value.some((v) => v !== value[0])

      if(isDifferent) {
        result.push(key)
      }
    }
    return result
  })

  function getCheckIcon(value: boolean):string {
    return value ? "/src/assets/img/GreenCheckmark.svg" : "/src/assets/img/RedCrossIcon.svg"
  }

  onMounted(loadPhones);

</script>