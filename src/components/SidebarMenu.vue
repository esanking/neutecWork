<script setup>
import { ref, provide } from 'vue'
import SidebarItem from './SidebarItem.vue'
import data from '../data/sidebarData.json'

const selectedPath = ref([])

function findPath(tree, key, path = []) {
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

function setSelected(key) {
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
  background: #000;
  padding: 12px;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.2);
  overflow-y: auto;
}
</style>
