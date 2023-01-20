<script setup>
import { ref, computed } from "vue";

const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

//Declaring a winner //
function CalculateWinner(squares) {
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
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

const winner = computed(() => CalculateWinner(board.value.flat()));

const MakeMove = (x, y) => {
  if (winner.value) return;

  if (board.value[x][y] != "") return;
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

<template>
  <main class="pt-5 text-center dark:bg-secondary text-light">
    <h1 class="mb-5 fs-1 fw-bold">TIC TAC TOE</h1>

    <h3 class="fs-3 mb-3">Player {{ player }}'s turn</h3>
    <div class="d-flex flex-column align-items-center mb-5">
      <div v-for="(row, x) in board" :key="x" class="d-flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="MakeMove(x, y)"
          class="board"
          :style="`color:${
            cell === 'X' ? 'pink' : 'aqua'
          } `"
        >
          {{ cell === "X" ? "X" : cell === "O" ? "O" : "" }}
          <!-- :class="`border border-light w-20 h-20 d-flex justify-content-center align-items-center material-icons-outlined fs-1 cursor-pointer`" -->
        </div>
      </div>
    </div>
    <h2 v-if="winner" class="winner fs-1 fw-bolder mb-5"
    :style="`color:${winner === 'X' ? 'pink' : 'aqua'}; text-shadow: 0px 0px 30px ${winner === 'X' ? 'pink' : 'aqua'} `">
      Player '{{ winner }}' wins!
    </h2>
    <button @click="ResetGame" class="btn btn-secondary p-2 rounded text-light">
      RESET GAME
    </button>
  </main>
</template>

<style scoped>
.board {
  border: 2px solid white;
  width: 5rem;
  height: 5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 3rem;
  line-height: 2.5rem;
  cursor: pointer;
}
.board:hover {
  background: rgb(75, 76, 105);
}
</style>
