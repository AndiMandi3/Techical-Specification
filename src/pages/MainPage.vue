<script setup lang="ts">
import MainHeader from "../components/MainHeader.vue"
import SmartphonesHeader from "../components/SmartphonesHeader.vue"
import SmartphoneTable from "../components/SmartphoneTable.vue"
import { SPECIFICATION_LABELS } from "../consts/specificationLabels"

import { onMounted, ref, computed } from "vue"
import type { phoneType } from "../types/phoneType.ts"

const phones = ref<phoneType[]>([])
const itemOfPhone = ref(3)
const showOnlyDiffs = ref(false)

const countsPhone = computed(() => {
  const n = phones.value.length
  return Array.from({ length: n - 1 }, (_, i) => i + 2)
})

const includesSpecKeys = computed(() => {
  if (!showOnlyDiffs.value) return specKeys
  return specKeys.filter(key => diffsKeys.value.includes(key))
})

function setItemsOfPhone(count: number): void {
  itemOfPhone.value = count
}

const visiblePhones = computed(() => {
  return phones.value.slice(0, itemOfPhone.value)
})

const remainingPhones = computed(() =>
    phones.value.filter(phone => !visiblePhones.value.some(vp => vp.id === phone.id))
)

async function loadPhones(): Promise<void> {
  try {
    const url = import.meta.env.VITE_URL_API
    const json = await fetch(url);
    phones.value = await json.json();
  } catch {
    phones.value = []
  }
}

const specKeys = Object.keys(SPECIFICATION_LABELS)

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

onMounted(loadPhones)
</script>

<template>
  <MainHeader/>
  <div class="smartphones-menu d-flex">
    <SmartphonesHeader
        :countsPhone="countsPhone"
        :current-count="itemOfPhone"
        @update-count="setItemsOfPhone"
    />
    <SmartphoneTable
        v-if="phones.length"
        v-model:show-only-diffs="showOnlyDiffs"
        :visiblePhones="visiblePhones"
        :includesSpecKeys="includesSpecKeys"
        :specificationLabels="SPECIFICATION_LABELS"
        :remaining-phones="remainingPhones"
    />
  </div>
</template>

<style scoped lang="scss">
.smartphones-menu {
  padding: 20px 165px 0 165px;
  flex-direction: column;
}
</style>