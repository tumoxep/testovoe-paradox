<script setup>
import { ref, watch } from 'vue';

import DocList from './components/DocList.vue';
import SearchInput from './components/SearchInput.vue';

const docs = ref([
  {
    id: 0,
    name: 'Обязательные для всех',
    description: 'Документы, обязательные для всех сотрудников без исключения',
    show: true,
    docs: [
      {
        id: 0,
        name: 'Паспорт',
        show: true,
      },
      {
        id: 1,
        name: 'ИНН',
        show: true,
      },
    ],
  },
  {
    id: 1,
    name: 'Обязательные для трудоустройства',
    description: 'Документы, без которых невозможно трудоустройство человека на какую бы то ни было должность в компании вне зависимости от граж',
    show: true,
    docs: [],
  },
  {
    id: 3,
    name: 'Специальные',
    description: 'Документы, обязательные для всех сотрудников без исключения',
    show: true,
    docs: [],
  },
  {
    id: 2,
    name: 'Тестовое задание кандидата',
    description: 'Россия, Белоруссия, Украина, администратор филиала, повар-сушист, повар-пиццмейкер, повар горячего цеха',
    show: true,
  },
  {
    id: 3,
    name: 'Трудовой договор',
    show: true,
  },
  {
    id: 4,
    name: 'Мед. книжка',
    show: true,
  },
]);

const searchText = ref('');

function toggleCollapse(category) {
  const el = docs.value.find(el => el.id === category.id);
  el.expanded = !el.expanded;
}

function updateDocs(newValue, isCategorized) {
  if (isCategorized) {
    docs.value = [
      ...newValue,
      ...docs.value.filter(el => el.docs === undefined),
    ];
  } else {
    docs.value = [
      ...docs.value.filter(el => el.docs !== undefined),
      ...newValue,
    ];
  }
}

watch(searchText, newSearchText => {
  if (!newSearchText?.length) {
    docs.value = docs.value.map(el => {
      if (el.docs?.length) {
        el.docs = el.docs.map(subEl => Object.assign({}, subEl, { show: true }));
      }
      return Object.assign({}, el, { show: true });
    });
    return;
  }
  docs.value = docs.value.map(el => {
    if (el.docs?.length) {
      el.docs = el.docs.map(subEl => Object.assign({}, subEl, { show: subEl.name.includes(newSearchText) }));
    }
    return Object.assign({}, el, { show: el.name.includes(newSearchText) });
  });
})
</script>

<template>
  <div class="app">
    <h1>Документы</h1>
    <SearchInput class="app__search-input" v-model="searchText" />
    <DocList
      :docs="docs"
      :searchText="searchText"
      @toggleCollapse="toggleCollapse"
      @updateDocs="updateDocs"
    />
  </div>
</template>

<style scoped>
  .app {
    margin: 30px;
  }
  h1 {
    font-style: normal;
    font-weight: bold;
    font-size: 22px;
    line-height: 108%;
    color: #000000;
    margin-top: 0;
    margin-bottom: 29px;
  }
  .app__search-input {
    margin-bottom: 19px;
  }
</style>
