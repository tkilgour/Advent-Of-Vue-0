<template>
  <div class="grid grid-cols-3 grid-rows-3 bg-gray-dark gap-0.5">
    <GameTile v-for="(tile, index) in tiles" :key="index" @click="() => handleClick(index)">
      {{ tile }}
    </GameTile>
  </div>
  <p class="pt-8 text-2xl">It's {{ playerTurn }}'s turn.</p>
</template>

<script setup>
import { ref, computed } from 'vue'
import GameTile from './GameTile.vue'

const boardState = ref([
  ['X', '', 'O'],
  ['', 'O', ''],
  ['', '', 'X'],
])
const tiles = computed(() => boardState.value.flat())

const playerTurn = ref(Math.random() > 0.5 ? 'X' : 'O')

const handleClick = index => {
  console.log(index)
  const row = Math.floor(index / 3)
  const col = index % 3
  boardState[row][col] = playerTurn
}
</script>
