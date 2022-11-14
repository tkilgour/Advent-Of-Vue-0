<template>
  <div
    :class="classList"
    class="absolute mx-auto my-auto inset-0 h-full w-0.5 bg-green transition-opacity duration-500"
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
  let classString = ''
  const positionDelta = props.position[1] - props.position[0]
  const firstPos = props.position[0]

  firstPos === null ? (classString = 'sr-only opacity-0 ') : (classString = 'opacity-100 ')

  // vertical line
  if (positionDelta === 6) {
    if (firstPos === 0) classString += 'ml-[16.2%]'
    if (firstPos === 2) classString += 'ml-[83.8%]'
  }
  // horizontal line
  if (positionDelta === 2) {
    if (firstPos === 0) classString += 'rotate-90 -mt-[33.1%]'
    if (firstPos === 3) classString += 'rotate-90'
    if (firstPos === 6) classString += 'rotate-90 -mb-[34%]'
  }
  // diagonal line
  if (positionDelta % 4 === 0) {
    if (firstPos === 0) classString += '-rotate-45 h-[130%]'
    if (firstPos === 2) classString += 'rotate-45 h-[130%]'
  }

  return classString
})
</script>
