<script setup lang="ts">
import MainHeader from "../components/MainHeader.vue"
import SmartphonesHeader from "../components/SmartphonesHeader.vue"
import SmartphoneTable from "../components/SmartphoneTable.vue";

import {onMounted, ref, computed} from "vue"
import type {phoneType} from "../types/phoneType.ts"

const phones = ref<phoneType[]>([])
const itemOfPhone = ref<number>(3)
const showOnlyDiffs = ref<boolean>(false)

const visiblePhones = computed(() => {
  return phones.value.slice(0, itemOfPhone.value)
})

const slicedCount = computed(() => {
  return phones.value.slice(1)
})

const includesSpecKeys = computed(() => {
  if (!showOnlyDiffs.value) return specKeys
  return specKeys.filter(key => diffsKeys.value.includes(key))
})

const notShowedAllPhones = computed(() => itemOfPhone.value < 6 )

function setItemsOfPhone(count: number): void {
  itemOfPhone.value = count
}

const booleanSpecKeys = ["nfc", "esim", "wirelessCharging"]

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
console.log(specKeys)

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

function


function getCheckIcon(value: boolean):string {
  return value ? "/src/assets/img/GreenCheckmark.svg" : "/src/assets/img/RedCrossIcon.svg"
}

function formatValue(key: string, value: unknown): string {
  if (key === "screenSize") return `${value}″`
  if (key === "fps") return `${value} Гц`
  if (key === "memorySize") return `${value} ГБ`
  if (key === "price") return `${value} ₽`
  return String(value)
}

onMounted(loadPhones)

</script>

<template>
  <MainHeader/>
  <!-- SmartPhonesView -->
  <div class="smartphones-menu d-flex">
    <SmartphonesHeader
        :sliced-phones="slicedCount"
        :current-count="itemOfPhone"
        @update-count="setItemsOfPhone"
    />
    <SmartphoneTable
        :visiblePhones="visiblePhones"
        v-model:showOnlyDiffs=showOnlyDiffs
        :includesSpecKeys="includesSpecKeys"
        :specificationLabels="specificationLabels"
        :booleanSpecKeys="booleanSpecKeys"
        :getCheckIcon="getCheckIcon"
        :formatValue="formatValue"
        :notShowedAllPhones="notShowedAllPhones"
    />
  </div>
</template>

<style scoped lang="scss">

.smartphones-menu {
  padding: 20px 165px 0 165px;
  flex-direction: column;
}




</style>