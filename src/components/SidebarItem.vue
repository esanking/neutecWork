<script setup>
import { inject, computed } from 'vue'

const props = defineProps({
  item: Object,
  level: Number,
})

const selectedPath = inject('selectedPath')
const setSelected = inject('setSelected')

const shouldExpand = computed(() => selectedPath.value.includes(props.item.key))
</script>

<template>
  <li>
    <div
      :style="{
        color: selectedPath.includes(item.key) ? 'yellow' : '#fff',
        fontWeight: selectedPath.includes(item.key) ? 'bold' : 'normal',
        cursor: 'pointer',
        padding: '2px 4px',
      }"
      @click="setSelected(item.key)"
    >
      {{ item.text }}
    </div>

    <ul v-if="item.children && shouldExpand">
      <SidebarItem
        v-for="child in item.children"
        :key="child.key"
        :item="child"
        :level="level + 1"
      />
    </ul>
  </li>
</template>
