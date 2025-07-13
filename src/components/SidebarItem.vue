<script setup lang="ts">
import { inject, computed, type Ref } from 'vue'
import type { SidebarItemType } from '@/types/sidebar'

const props = defineProps<{
  item: SidebarItemType
  level: number
}>()

const selectedPath = inject<Ref<string[]>>('selectedPath')!
const setSelected = inject<(key: string) => void>('setSelected')!

const shouldExpand = computed(() => selectedPath.value.includes(props.item.key))
</script>

<template>
  <li>
    <div
      :style="{
        color: selectedPath.includes(item.key) ? 'yellow' : '#fff',
        fontWeight: selectedPath.includes(item.key) ? 'bold' : 'normal',
        cursor: 'pointer',
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
