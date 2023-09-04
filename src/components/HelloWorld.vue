<template>
  <div class="container">
    <div>
      <h1 class="game-title">Tic Tac Toe</h1>
      <p class="current-player">Turno: {{ currentPlayer }}</p>
    </div>
    <div class="board">
      <div
        class="cell"
        v-for="(cell, index) in board"
        :key="index"
        @click="makeMove(index)"
        :class="{ 'player-x': currentPlayer === 'X', 'player-o': currentPlayer === 'O' }"
      >
        {{ cell }}
      </div>
    </div>
    <div>
      <p class="announcement">{{ announcement }}</p>
      <button @click="resetBoard">Restart</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      board: Array(9).fill(""),
      currentPlayer: "X",
      isGameActive: true,
      announcement: "",
    };
  },
  methods: {
    makeMove(index) {
      if (this.isGameActive && !this.board[index]) {
        this.$set(this.board, index, this.currentPlayer);
        if (this.checkWin() === this.currentPlayer) {
          this.announcement = `Player ${this.currentPlayer} wins!`;
          this.isGameActive = false;
        } else if (this.board.indexOf("") === -1) {
          this.announcement = "It's a tie!";
          this.isGameActive = false;
        } else {
          this.currentPlayer = this.currentPlayer === "X" ? "O" : "X";
        }
      }
    },
    resetBoard() {
      this.board = Array(9).fill("");
      this.currentPlayer = "X";
      this.announcement = "";
      this.isGameActive = true;
    },
    checkWin() {
      const winPatterns = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];
      for (const pattern of winPatterns) {
        const [a, b, c] = pattern;
        if (
          this.board[a] &&
          this.board[a] === this.board[b] &&
          this.board[a] === this.board[c]
        ) {
          return this.board[a];
        }
      }
      return null;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  text-align: center;
  background-color: #748e71;
}

.game-title {
  font-size: 36px;
  color: #ffffff;
  margin-bottom: 20px;
}

.current-player {
  font-size: 20px;
  color: #ffffff;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  grid-gap: 5px;
  margin-top: 20px;
}

.cell {
  width: 100px;
  height: 100px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
  cursor: pointer;
  background-color: #9ebba9;
  transition: background-color 0.3s ease;
  border-radius: 10px;
}

.cell:hover {
  background-color: #bbc8ab;
}

.cell.player-x:hover {
  background-color: rgb(150, 99, 99); /* Rojo para X */
}

.cell.player-o:hover {
  background-color: rgb(91, 91, 137); /* Azul para O */
}

.announcement {
  font-size: 24px;
  color: #f00;
  margin-top: 20px;
}

button {
  font-size: 18px;
  padding: 10px 20px;
  background-color: #009688;
  color: #fff;
  border: none;
  cursor: pointer;
  margin-top: 20px;
}

button:hover {
  background-color: #00766c;
}
</style>
