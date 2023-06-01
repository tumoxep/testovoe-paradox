<script setup>
import { computed } from 'vue';

import IconCross from './icons/IconCross.vue';
import IconSearch from './icons/IconSearch.vue';

const props = defineProps(['modelValue']);
defineEmits(['update:modelValue']);
const showCross = computed(() => props.modelValue?.length > 0);
</script>

<template>
  <div class="search-input">
    <IconSearch class="search-input__search-icon" />
    <input
      class="search-input__input"
      placeholder="Поиск"
      :value="modelValue"
      @input="$emit('update:modelValue', $event.target.value)"
    />
    <IconCross v-if="showCross" class="search-input__cross-icon" />
  </div>
</template>

<style scoped>
  .search-input {
    border-bottom: 2px solid #8E9CBB;
    padding-bottom: 12px;
    display: flex;
    align-items: center;
    max-width: 564px;
  }
  .search-input__search-icon {
    color: #8E9CBB;
    margin-right: 12px;
    width: 13.66px;
    height: 14.24px;
  }
  .search-input__cross-icon {
    color: #FF238D;
    width: 11.07px;
    height: 11.04px;
    cursor: pointer;
  }
  .search-input__input {
    border: none;
    font-size: 15px;
    flex-grow: 1;
    font-family: 'Fira Sans', sans-serif;
    line-height: 16px;
  }
  .search-input__input:focus{
    outline: none;
  }
  .search-input__input::placeholder {
    font-style: italic;
  }
</style>
