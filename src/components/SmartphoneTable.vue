<template>
  <div
      class="specifications-table"
      :style="`--phone-count: ${visiblePhones.length}`"
  >
    <div class="specifications-table__cell is-header">
      <input
          type="checkbox"
          name="specifications-table__differents-mobiles"
          :checked="showOnlyDiffs"
          @change="$emit('update:showOnlyDiffs', ($event.target as HTMLInputElement).checked)"
      >
      <label for="specifications-table__checkbox-differents" class="blue">Показать различия</label>
    </div>
    <SmartphoneCard
        v-for="phone in visiblePhones"
        :key="'head-' + phone.id"
        :phone="phone"
        :show-switcher="notShowedAllPhones"
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
          v-for="phone in visiblePhones"
          :key="phone.id + '-' + specKey"
          :value="phone[specKey]"
          :specKey="specKey"
          :isBoolean="booleanSpecKeys.includes(specKey)"
          :getCheckIcon="getCheckIcon"
          :formatValue="formatValue"
      />
    </template>
  </div>
</template>

<script setup lang="ts">
import type {phoneType} from "../types/phoneType.ts";
import SmartphoneCard from "./SmartphoneCard.vue";
import SpecCell from "./SpecCell.vue";

defineProps<{
  visiblePhones: phoneType[]
  showOnlyDiffs: boolean
  notShowedAllPhones: boolean
  includesSpecKeys: string[]
  specificationLabels: Record<string, string>
  booleanSpecKeys: string[]
  getCheckIcon: (value: boolean) => string
  formatValue: (key: string, value: unknown) => string
}>()

defineEmits(["update:showOnlyDiffs"])
</script>

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