<template>
  <div class="grid grid-cols-3 grid-rows-3 bg-gray-dark gap-0.5 relative">
    <WinLine :position="winLinePosition" />
    <GameTile
      v-for="(tile, index) in tiles"
      :key="index"
      :disabled="tile.length || winner"
      @click="() => handleClick(index)"
    >
      <div class="w-full h-full grid place-items-center group">
        <span v-if="!(tile || winner)" class="transition-opacity opacity-0 group-hover:opacity-30">
          {{ playerTurn }}
        </span>
        {{ tile }}
      </div>
    </GameTile>
  </div>
  <p class="pt-8 text-2xl">{{ message }}</p>
  <button v-if="winner" class="text-lg px-4 py-2 mt-4 bg-green rounded" @click="resetGame">Play again!</button>
</template>

<script setup>
import { ref, computed } from 'vue'
import GameTile from './GameTile.vue'
import WinLine from './WinLine.vue'

const boardState = ref(null)
const tiles = computed(() => boardState.value.flat())
const playerTurn = ref(Math.random() > 0.5 ? 'X' : 'O')
const winner = ref(null)
const winLinePosition = ref([null, null])

const checkWinState = () => {
  // check rows for all playerTurn
  boardState.value.forEach((row, index) => {
    if (row.every(item => item === playerTurn.value)) {
      winner.value = playerTurn.value
      winLinePosition.value = [index * 3, index * 3 + 2]
    }
  })

  // check cols for all playerTurn
  for (let i = 0; i < 3; i++) {
    if (boardState.value.every(row => row[i] === playerTurn.value)) {
      winner.value = playerTurn.value
      winLinePosition.value = [i, i + 6]
    }
  }

  // check diagonals for all playerTurn
  if (boardState.value.every((row, index) => row[index] === playerTurn.value)) {
    winner.value = playerTurn.value
    winLinePosition.value = [0, 8]
  }
  if (boardState.value.every((row, index) => row[2 - index] === playerTurn.value)) {
    winner.value = playerTurn.value
    winLinePosition.value = [2, 6]
  }

  // check draw
  if (!boardState.value.some(row => row.some(item => item === ''))) winner.value = 'draw'
}

const handleClick = index => {
  const row = Math.floor(index / 3)
  const col = index % 3
  boardState.value[row][col] = playerTurn.value

  checkWinState()

  playerTurn.value === 'X' ? (playerTurn.value = 'O') : (playerTurn.value = 'X')
}

const message = computed(() => {
  if (winner.value === 'draw') {
    return `It's a ${winner.value}.`
  } else if (winner.value) {
    return `${winner.value} has won!`
  } else {
    return `It's ${playerTurn.value}'s turn.`
  }
})

const resetGame = () => {
  winner.value = null
  winLinePosition.value = [null, null]

  boardState.value = [
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ]
}
resetGame()
</script>
