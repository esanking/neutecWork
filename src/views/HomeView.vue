<script setup lang="ts">
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
      <Transition name="SidebarMenu">
        <SidebarMenu v-if="isSidebarOpen" @close="closeSidebar" />
      </Transition>
    </div>
  </div>
</template>

<style scoped>
.home {
  width: 100%;
  height: 100vh;
  display: flex;
  background-color: gray;
  .navBar {
    width: 100%;
    height: 50px;
    color: white;
    background-color: #fff;
    display: flex;
    align-items: center;
    justify-content: end;
    padding: 0 20px;
  }
  .SidebarMenu-enter-active,
  .SidebarMenu-leave-active {
    transition: all 0.3s ease;
  }

  .SidebarMenu-enter-from,
  .SidebarMenu-leave-to {
    width: 0;
  }
}
</style>
