<template>
  <div>
    <b-container>
      <div class="gameContainer">
        <table>
          <tr
            v-for="(row, rowIndex) in matrix"
            :key="rowIndex"
          >
            <td
              v-for="(col,coulumnIndex) in row"
              :key="rowIndex+coulumnIndex"
              :style="
                [winningSelection(`${rowIndex}${coulumnIndex}`)?{'background':'greenyellow'}:'']"
              :class="[!matrix[rowIndex][coulumnIndex] &&
                !playerOneWon && !playerTwoWon ?'poniter':'']"
              @click="e=> !matrix[rowIndex][coulumnIndex] && !playerOneWon && !playerTwoWon &&
                clickCell(rowIndex,coulumnIndex,e)"
            >
              {{ matrix[rowIndex][coulumnIndex]?matrix[rowIndex][coulumnIndex]:'' }}
            </td>
          </tr>
        </table>
      </div>
      <br/>
      <button @click="resetGame()">
        Reset
      </button>
      <div v-if="playerOneWon">
        Player One won!!
      </div>
      <div v-if="playerTwoWon">
        Player Two won!!
      </div>
      <div v-if="draw">
        Draw!!
      </div>
    </b-container>
  </div>
</template>

<script>
export default {
  name: 'TicTacToe',
  data() {
    return {
      matrix: [
        [null, null, null],
        [null, null, null],
        [null, null, null],
      ],
      previousSelection: null,
      playerOneSelectedCells: [],
      winningData: [],
      playerOneWon: false,
      playerTwoSelectedCells: [],
      playerTwoWon: false,
      winningCells: [
        ['00', '01', '02'],
        ['00', '11', '22'],
        ['00', '10', '20'],
        ['01', '11', '21'],
        ['02', '12', '22'],
        ['02', '11', '20'],
        ['10', '11', '12'],
        ['20', '21', '22'],
      ],
    };
  },
  computed: {
    playerOneWin() {
      let match = false;
      match = this.winningCells.some((com) => {
        const test = com.every((val) => this.playerOneSelectedCells.includes(val));
        if (test) {
          return true;
        }
        return false;
      });
      return match;
    },
    playerTwoWin() {
      let match = false;
      match = this.winningCells.some((com) => {
        const test = com.every((val) => this.playerTwoSelectedCells.includes(val));
        if (test) {
          return true;
        }
        return false;
      });
      return match;
    },
    draw() {
      return this.playerOneSelectedCells.length === 5 && !this.playerOneWon && !this.playerTwoWon;
    },
  },
  methods: {
    clickCell(row, col) {
      // The first selection will X for a game
      if (!this.previousSelection || this.previousSelection === 'O') {
        this.previousSelection = 'X';
        this.playerOneSelectedCells.push(`${row}${col}`);
      } else {
        this.previousSelection = 'O';
        this.playerTwoSelectedCells.push(`${row}${col}`);
      }
      // Updating the game matrix with selected value
      const newRow = this.matrix[row].slice(0);
      newRow[col] = this.previousSelection;
      this.$set(this.matrix, row, newRow);
      this.checkGameStatus();
    },
    checkGameStatus() {
      let playerOneStatus = false;
      let playerTwoStatus = false;
      playerOneStatus = this.winningCells.some((com) => {
        const isPlayerWon = com.every((val) => this.playerOneSelectedCells.includes(val));
        if (isPlayerWon) {
          // When player wins, winning combination is saved to winningData to mark cells
          this.winningData = com;
          return true;
        }
        return false;
      });

      playerTwoStatus = this.winningCells.some((com) => {
        const isPlayerWon = com.every((val) => this.playerTwoSelectedCells.includes(val));
        if (isPlayerWon) {
          // When player wins, winning combination is saved to winningData to mark cells
          this.winningData = com;
          return true;
        }
        return false;
      });
      this.playerOneWon = playerOneStatus;
      this.playerTwoWon = playerTwoStatus;
    },
    resetGame() {
      this.matrix = [
        [null, null, null],
        [null, null, null],
        [null, null, null],
      ];
      this.previousSelection = null;
      this.playerOneSelectedCells = [];
      this.playerTwoSelectedCells = [];
      this.playerOneWon = false;
      this.playerTwoWon = false;
      this.winningData = [];
    },
    // For styling winning cells
    winningSelection(index) {
      return this.winningData.includes(index);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table {
  border-collapse: collapse;
  border: 1px solid black;
  table-layout: fixed;
  width: 180px;
}
th,td {
  border: 1px solid black;
  font-size: 36px;
}
tr{
  min-height: 56px;
  height: 56px;
}
.success{
  background: greenyellow;
}
.poniter{
  cursor: pointer;
}
.gameContainer{
  width: 15%;
  margin: auto;
}
</style>
