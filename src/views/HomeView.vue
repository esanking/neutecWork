<script setup>
import { ref } from 'vue'
import SidebarMenu from '../components/SidebarMenu.vue'

const isSidebarOpen = ref(false)

function toggleSidebar() {
  isSidebarOpen.value = !isSidebarOpen.value
}

function closeSidebar() {
  isSidebarOpen.value = false
}
</script>

<template>
  <div @click="closeSidebar" class="home">
    <div class="navBar">
      <button @click.stop="toggleSidebar">開啟選單</button>
      <SidebarMenu v-if="isSidebarOpen" @close="closeSidebar" />
    </div>
  </div>
</template>

<style scoped>
.home {
  width: 100%;
  height: 100vh;
  display: flex;
  .navBar {
    width: 100%;
    height: 50px;
    color: white;
    display: flex;
    align-items: center;
    justify-content: end;
    padding: 0 20px;
  }
}
</style>
