<script setup lang="ts">
import {computed, ref} from "vue";
import type { phoneType } from "../types/phoneType.ts"
import SmartphoneCard from "./SmartphoneCard.vue"
import SpecCell from "./SpecCell.vue"
import { BOOLEAN_SPEC_KEYS } from "../consts/booleanSpecKeys";

const { visiblePhones } = defineProps<{
  visiblePhones: phoneType[]
  showOnlyDiffs: boolean
  includesSpecKeys: string[]
  specificationLabels: Record<string, string>
  remainingPhones: phoneType[]
}>()

const isNotShowedAllPhones = computed(() => visiblePhones.length < 6 )

const showOnlyDiff = defineModel("showOnlyDiffs")
const newVisiblePhones = ref(visiblePhones)
</script>

<template>
  <div
      class="specifications-table"
      :style="`--phone-count: ${visiblePhones.length}`"
  >
    <div class="specifications-table__cell is-header">
      <input
          type="checkbox"
          name="specifications-table__differents-mobiles"
          v-model="showOnlyDiff"
      >
      <label for="specifications-table__checkbox-differents" class="blue">Показать различия</label>
    </div>
    <SmartphoneCard
        v-for="(phone, index) in newVisiblePhones"
        :key="phone.name"
        v-model="newVisiblePhones[index]"
        :show-switcher="isNotShowedAllPhones"
        :remaining-phones="remainingPhones"
    />
    <template
        v-for="specKey in includesSpecKeys"
        :key="specKey"
        class="specifications-table__row"
    >
      <div class="specifications-table__cell is-label">
        {{ specificationLabels[specKey] }}
      </div>
      <SpecCell
          v-for="phone in newVisiblePhones"
          :key="phone.id"
          :value="phone[specKey]"
          :specKey="specKey"
          :isBoolean="BOOLEAN_SPEC_KEYS.includes(specKey)"
      />
    </template>
  </div>
</template>

<style scoped lang="scss">
.specifications-table {
  display: grid;
  grid-template-columns: auto repeat(var(--phone-count), 1fr);
  gap: 8px;
  width: 100%;
  font-family: "Roboto Medium", sans-serif;
  margin-top: 20px;

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

    &.is-header {
      font-weight: 600;
      border: none;
      background: white;
    }

    &.is-label {
      background: #f5f5f5;
      font-weight: 500;
      justify-content: flex-start;
    }

  }

  &__cell-wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
  }
}
</style>