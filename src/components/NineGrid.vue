<script setup lang="ts">
import { ref, onMounted } from 'vue'
import type { CSSProperties } from 'vue'

function ballStyle(i: number): CSSProperties {
  const gap = 6
  const cellWidth = (window.innerWidth - gap * 2) / 3
  const cellHeight = 100
  const ballSize = 30
  const row = Math.floor((i - 1) / 3)
  const col = (i - 1) % 3

  const top = row * (cellHeight + gap) + (cellHeight - ballSize) / 2
  const left = col * (cellWidth + gap) + 10

  return {
    position: 'absolute',
    top: `${top}px`,
    left: `${left}px`,
  }
}

const balls = ref<HTMLDivElement[]>([])

onMounted(() => {
  balls.value.forEach((ball) => {
    animateBall(ball)
  })
})

function animateBall(el: HTMLElement) {
  let pos = 0
  const max = (window.innerWidth / 3) * 2
  const speed = 3

  function step() {
    if (pos >= max) return
    pos += speed
    el.style.transform = `translateX(${pos}px)`
    requestAnimationFrame(step)
  }

  step()
}
</script>

<template>
  <div class="grid-section">
    <div class="grid-container">
      <template v-for="i in 9" :key="i">
        <div class="grid-item" :class="{ flash: [3, 5, 9].includes(i) }"></div>
        <div v-if="[1, 3, 7, 9].includes(i)" class="ball" ref="balls" :style="ballStyle(i)" />
      </template>
    </div>
  </div>
</template>

<style scoped>
@keyframes flash {
  0%,
  100% {
    opacity: 1;
    filter: brightness(1.5);
  }
  50% {
    opacity: 0.6;
    filter: brightness(1.2);
  }
}

.grid-section {
  flex: 1;
  background-color: #555;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
}

.grid-container {
  position: relative;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 6px;
  width: 100%;
  max-width: 390px;
}

.grid-item {
  position: relative;
  height: 100px;
  border: 2px solid black;
  background: radial-gradient(circle, rgba(113, 81, 95, 1) 81%, rgba(0, 0, 0, 1) 100%);
}

.flash {
  animation: flash 0.6s infinite ease-in-out;
}

.ball {
  width: 30px;
  height: 30px;
  background-color: #a5f12b;
  border-radius: 50%;
  z-index: 1;
}
</style>
