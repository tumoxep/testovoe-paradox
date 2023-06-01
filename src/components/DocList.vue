<script setup>
import { computed } from 'vue';
import draggable from 'vuedraggable';

import IconPencil from './icons/IconPencil.vue';
import IconTrash from './icons/IconTrash.vue';
import IconArrowsVertical from './icons/IconArrowsVertical.vue';
import IconArrowUp from './icons/IconArrowUp.vue';
import IconArrowDown from './icons/IconArrowDown.vue';

const props = defineProps(['categories', 'docs']);
const emit = defineEmits(['toggleCollapse', 'updateDocs']);

const categories = computed({
  get() {
    return props.docs.filter(el => el.docs !== undefined);
  },
  set(newValue) {
    emit('updateDocs', newValue, true);
  }
});
const uncategorized = computed({
  get() {
    return props.docs.filter(el => el.docs === undefined);
  },
  set(newValue) {
    emit('updateDocs', newValue, false);
  }
});

function collapseClick(category) {
  emit('toggleCollapse', category);
}
</script>

<template>
  <div class="doc-list">
    <div class="doc-list__categories">
      <draggable
        v-model="categories"
        item-key="id"
        handle=".doc-list__icon-arrows"
        drag-class="doc-list__item-drag"
        ghost-class="doc-list__item-ghost"
        :force-fallback="true"
      >
        <template #item="{element}">
          <div class="doc-list__category" v-show="element.show">
            <div class="doc-list__item">
              <div class="doc-list__category-collapse" @click="collapseClick(element)">
                <IconArrowUp v-if="element.expanded" />
                <IconArrowDown v-else />
              </div>
              <span class="doc-list__item-name">{{ element.name }}</span>
              <span class="doc-list__item-description">{{ element.description }}</span>
              <div class="doc-list__item-actions">
                <IconPencil class="doc-list__icon-pencil" />
                <IconTrash class="doc-list__icon-trash" />
                <IconArrowsVertical class="doc-list__icon-arrows" />
              </div>
            </div>
            <Transition name="collapse">
            <div class="doc-list__category-list" v-if="element.expanded || element.docs.length === 0">
              <draggable
                v-model="element.docs"
                item-key="id"
                handle=".doc-list__icon-arrows"
                drag-class="doc-list__item-drag"
                ghost-class="doc-list__item-ghost"
                :force-fallback="true"
                group="doc-list"
              >
                <template #item="{element}">
                  <div class="doc-list__item" v-show="element.show">
                    <span class="doc-list__item-name">{{ element.name }}</span>
                    <span class="doc-list__item-description">{{ element.description }}</span>
                    <div class="doc-list__item-actions">
                      <IconPencil class="doc-list__icon-pencil" />
                      <IconTrash class="doc-list__icon-trash" />
                      <IconArrowsVertical class="doc-list__icon-arrows" />
                    </div>
                  </div>
                </template>
              </draggable>
            </div>
            </Transition>
          </div>
        </template>
      </draggable>
    </div>
    <draggable
      v-model="uncategorized"
      item-key="id"
      handle=".doc-list__icon-arrows"
      drag-class="doc-list__item-drag"
      ghost-class="doc-list__item-ghost"
      :force-fallback="true"
      :fallback-on-body="true"
      fallback-class="doc-list__item-drag"
      group="doc-list"
    >
      <template #item="{element}">
        <div class="doc-list__item" v-show="element.show">
          <span class="doc-list__item-name">{{ element.name }}</span>
          <span class="doc-list__item-description">{{ element.description }}</span>
          <div class="doc-list__item-actions">
            <IconPencil class="doc-list__icon-pencil" />
            <IconTrash class="doc-list__icon-trash" />
            <IconArrowsVertical class="doc-list__icon-arrows" />
          </div>
        </div>
      </template>
    </draggable>
  </div>
</template>

<style scoped>
  .doc-list {
    width: 100%;
    max-width: 1190px;
    white-space: nowrap;
  }
  .doc-list__item {
    display: flex;
    align-items: center;
    padding: 10px 17px;
    border: 1px solid #DFE4EF;
  }
  .doc-list__item-name {
    font-size: 13px;
    line-height: 14px;
    margin-right: 15px;
  }
  .doc-list__item-description {
    font-size: 11px;
    line-height: 12px;
    color: #8E9CBB;
    margin-right: 30px;
    overflow-x: hidden;
    flex-grow: 1;
  }
  svg {
    cursor: pointer;
  }
  .doc-list__icon-pencil {
    margin-right: 10px;
    color: #8E9CBB;
  }
  .doc-list__icon-trash {
    margin-right: 10px;
    color: #FF238D;
  }
  .doc-list__icon-arrows {
    color: #8E9CBB;
    cursor: grab;
  }
  .doc-list__category-collapse {
    border: 1px solid #D3D8DF;
    border-radius: 50%;
    width: 22px;
    height: 22px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-right: 14px;
    cursor: pointer;
    flex-shrink: 0;
  }
  .doc-list__category-collapse svg {
    color: #0066FF;
  }
  .doc-list__category:last-of-type {
    margin-bottom: 14px;
  }
  .doc-list__category > .doc-list__item {
    padding: 13px 16px;
  }
  .doc-list__category-list {
    padding-left: 16px;
    transform-origin: top;
  }
  .doc-list__item-drag {
    opacity: 1;
    box-shadow: 0px 3px 16px 0px #0066FFB2;
    background-color: white;
    height: unset !important;
  }
  .doc-list__item-drag .doc-list__icon-arrows {
    color: #0066FF;
  }
  .doc-list__item-ghost {
    height: 5px;
    padding: 0;
    background-color: #0066FF;
  }
  .doc-list__item-ghost * {
    display: none;
  }
</style>
