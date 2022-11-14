<template>
  <div
    v-if="position[0] !== null"
    :class="classList"
    class="absolute mx-auto my-auto inset-0 h-full w-0.5 bg-green"
  ></div>
</template>

<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  position: {
    type: Array,
    required: true,
  },
})

const classList = computed(() => {
  const positionDelta = props.position[1] - props.position[0]
  const firstPos = props.position[0]

  // vertical line
  if (positionDelta === 6) {
    if (firstPos === 0) return 'ml-[16.2%]'
    if (firstPos === 2) return 'ml-[83.8%]'
  }
  // horizontal line
  if (positionDelta === 2) {
    if (firstPos === 0) return 'rotate-90 -mt-[33.1%]'
    if (firstPos === 3) return 'rotate-90'
    if (firstPos === 6) return 'rotate-90 -mb-[34%]'
  }
  // diagonal line
  if (positionDelta % 4 === 0) {
    if (firstPos === 0) return '-rotate-45 h-[130%]'
    if (firstPos === 2) return 'rotate-45 h-[130%]'
  }
})
</script>
