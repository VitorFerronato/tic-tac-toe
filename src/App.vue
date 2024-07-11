<template>
  <main class="pt-8 text-center">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>

    <h3 v-if="!winner && !isDraw" class="text-xl mb-4">Player {{ player }}'s turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          :class="`border border-white w-24 h-24 hover:bg-gray-700 flex items-center justify-center material-icons-outlined text-4xl cursor-pointer ${
            cell === 'X' ? 'text-pink-500' : 'text-blue-400'
          }`"
          @click="MakeMove(x, y)"
        >
          {{ cell }}
        </div>
      </div>
    </div>

    <div class="text-center">
      <h2 v-if="winner" class="text-6xl font-bold mb-8">
        Player '{{ winner }}' wins!
      </h2>
      <h2 v-else-if="isDraw" class="text-6xl font-bold mb-8">
        It's a Draw!
      </h2>
      <button
        @click="ResetGame"
        class="px-4 py-2 bg-cyan-600 text-white rounded uppercase font-bold hover:text-indigo-100 duration-300"
      >
        Reset
      </button>
    </div>
  </main>
</template>

<script setup>
import { ref, computed } from "vue";

const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const CalculateWinner = (board) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];

    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      return board[a];
    }
  }

  return null;
};

const winner = computed(() => CalculateWinner(board.value.flat()));

const isDraw = computed(() => {
  return !winner.value && board.value.flat().every(cell => cell !== "");
});

const MakeMove = (x, y) => {
  if (winner.value || isDraw.value) return;

  if (board.value[x][y]) return;

  board.value[x][y] = player.value;

  player.value = player.value === "X" ? "O" : "X";
};

const ResetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
};
</script>

<style>
body {
  @apply bg-cyan-950 text-white;
}
</style>
