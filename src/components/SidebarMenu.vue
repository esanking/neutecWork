<script setup lang="ts">
import { ref, provide } from 'vue'
import type { Ref } from 'vue'
import SidebarItem from './SidebarItem.vue'
import data from '../data/sidebarData.json'
import type { SidebarItemType } from '@/types/sidebar'

const selectedPath: Ref<string[]> = ref([])

function findPath(tree: SidebarItemType[], key: string, path: string[] = []): string[] {
  for (const item of tree) {
    const currentPath = [...path, item.key]
    if (item.key === key) return currentPath
    if (item.children) {
      const result = findPath(item.children, key, currentPath)
      if (result.length) return result
    }
  }
  return []
}

function setSelected(key: string) {
  selectedPath.value = findPath(data, key)
}

provide('selectedPath', selectedPath)
provide('setSelected', setSelected)
</script>

<template>
  <div class="sidebar" @click.stop>
    <ul>
      <SidebarItem
        v-for="item in data"
        :key="item.key"
        :item="item"
        :level="0"
        :style="{
          backgroundColor: selectedPath.includes(item.key) ? 'gray' : '',
        }"
      />
    </ul>
  </div>
</template>

<style scoped>
.sidebar {
  position: absolute;
  top: 0;
  right: 0;
  width: 250px;
  height: 100vh;
  background: rgba(0, 0, 0, 0.9);
  padding: 12px;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.2);
  overflow-y: auto;
  z-index: 100;
}
</style>
