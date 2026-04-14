<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    images: string[]
    rows?: number
    cols?: number
    stagger?: number
    duration?: number
    gap?: string
  }>(),
  {
    rows: 4,
    cols: 6,
    stagger: 250,
    duration: 400,
    gap: '6px',
  },
)

type Entry = { src: string; order: number }

const cells = computed<Entry[][]>(() => {
  const total = props.rows * props.cols
  const grid: Entry[][] = Array.from({ length: total }, () => [])
  props.images.forEach((src, i) => {
    grid[i % total].push({ src, order: i })
  })
  return grid
})
</script>

<template>
  <div
    class="image-grid"
    :style="{
      gridTemplateColumns: `repeat(${cols}, 1fr)`,
      gridTemplateRows: `repeat(${rows}, 1fr)`,
      gap,
    }"
  >
    <div v-for="(cell, i) in cells" :key="i" class="image-grid-cell">
      <img
        v-for="entry in cell"
        :key="entry.order"
        v-motion
        :initial="{ opacity: 0, scale: 1.3, y: -24 }"
        :enter="{
          opacity: 1,
          scale: 1,
          y: 0,
          transition: {
            delay: entry.order * stagger,
            duration,
            type: 'spring',
            damping: 14,
            stiffness: 120,
          },
        }"
        :src="entry.src"
        :style="{ zIndex: entry.order }"
        class="image-grid-img"
      />
    </div>
  </div>
</template>

<style scoped>
.image-grid {
  display: grid;
  position: absolute;
  inset: 0;
}
.image-grid-cell {
  position: relative;
  overflow: hidden;
}
.image-grid-img {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: contain;
}
</style>
