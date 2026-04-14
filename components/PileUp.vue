<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    images: string[]
    maxOffset?: number
    maxRotation?: number
    width?: string
  }>(),
  {
    maxOffset: 80,
    maxRotation: 12,
    width: '320px',
  },
)

// Deterministic pseudo-random in [-1, 1] from a seed. Mulberry32-ish.
function rand(seed: number): number {
  let t = (seed * 2654435761) >>> 0
  t = (t ^ (t >>> 15)) * 2246822507
  t = (t ^ (t >>> 13)) * 3266489909
  t = t ^ (t >>> 16)
  return ((t >>> 0) / 0xffffffff) * 2 - 1
}

const items = computed(() =>
  props.images.map((src, i) => {
    const sign = i % 2 === 0 ? 1 : -1
    const rot = sign * (0.4 + Math.abs(rand(i * 3 + 1)) * 0.6) * props.maxRotation
    return {
      src,
      x: rand(i * 7 + 2) * props.maxOffset,
      y: rand(i * 11 + 5) * props.maxOffset,
      rot,
    }
  }),
)
</script>

<template>
  <div class="pileup" :style="{ width }">
    <img
      v-for="(item, i) in items"
      :key="i"
      v-click
      :src="item.src"
      class="pileup-img"
      :style="{
        transform: `translate(-50%, -50%) translate(${item.x}px, ${item.y}px) rotate(${item.rot}deg)`,
        zIndex: i,
      }"
    />
  </div>
</template>

<style scoped>
.pileup {
  position: relative;
  aspect-ratio: 1 / 1;
  margin: 0 auto;
}
.pileup-img {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 100%;
  height: auto;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.25);
  transition:
    transform 0.4s cubic-bezier(0.2, 0.8, 0.3, 1.2),
    filter 0.3s ease;
}
/* Dim any revealed image that has a later revealed sibling — i.e. something
   has been dropped on top of it. */
.pileup-img:not(.slidev-vclick-hidden):has(
    ~ .pileup-img:not(.slidev-vclick-hidden)
  ) {
  filter: brightness(0.55);
}
</style>
