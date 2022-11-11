<template>
  <div class="grid grid-cols-3 grid-rows-3 bg-gray-dark gap-0.5">
    <GameTile v-for="(tile, index) in tiles" :key="index">
      {{ tile }}
    </GameTile>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import GameTile from './GameTile.vue'

const boardState = ref([
  ['x', '', 'o'],
  ['', 'o', ''],
  ['', '', 'x'],
])

const tiles = computed(() => boardState.value.flat())
</script>
