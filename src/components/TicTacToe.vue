<template>
  <div class="outer-container">
    <h2 v-if="!(playerOneWon||playerTwoWon||draw)">
      Player {{ !previousSelection || previousSelection=='O'?1:2 }} Turn
    </h2>
    <div
      v-if="playerOneWon"
      class="blink"
    >
      <h2>
        Player One won !!
      </h2>
    </div>
    <div
      v-if="playerTwoWon"
      class="blink"
    >
      <h2>
        Player Two won !!
      </h2>
    </div>
    <div
      v-if="draw"
      class="blink"
    >
      <h2>
        Draw !!
      </h2>
    </div>
    <br>
    <div class="grid-container">
      <div
        class="grid-item top left"
        :class="[!matrix[0][0] &&
          !playerOneWon && !playerTwoWon ?'pointer':'']"
        @click="!matrix[0][0] && !playerOneWon && !playerTwoWon &&
          clickCell(0,0)"
      >
        <div :class="[winningSelection(`${0}${0}`)?'blink':'']">
          {{ matrix[0][0]?matrix[0][0]: '' }}
        </div>
      </div>
      <div
        class="grid-item top middle"
        :class="[!matrix[0][1] &&
          !playerOneWon && !playerTwoWon ?'pointer':'']"
        @click="!matrix[0][1] && !playerOneWon && !playerTwoWon &&
          clickCell(0,1)"
      >
        <div :class="[winningSelection(`${0}${1}`)?'blink':'']">
          {{ matrix[0][1]?matrix[0][1]: '' }}
        </div>
      </div>
      <div
        class="grid-item top right"
        :class="[!matrix[0][2] &&
          !playerOneWon && !playerTwoWon ?'pointer':'']"
        @click="!matrix[0][2] && !playerOneWon && !playerTwoWon &&
          clickCell(0,2)"
      >
        <div :class="[winningSelection(`${0}${2}`)?'blink':'']">
          {{ matrix[0][2]?matrix[0][2]: '' }}
        </div>
      </div>
      <div
        class="grid-item left"
        :class="[!matrix[1][0] &&
          !playerOneWon && !playerTwoWon ?'pointer':'']"
        @click="!matrix[1][0] && !playerOneWon && !playerTwoWon &&
          clickCell(1,0)"
      >
        <div :class="[winningSelection(`${1}${0}`)?'blink':'']">
          {{ matrix[1][0]?matrix[1][0]: '' }}
        </div>
      </div>
      <div
        class="grid-item"
        :class="[!matrix[1][1] &&
          !playerOneWon && !playerTwoWon ?'pointer':'']"
        @click="!matrix[1][1] && !playerOneWon && !playerTwoWon &&
          clickCell(1,1)"
      >
        <div :class="[winningSelection(`${1}${1}`)?'blink':'']">
          {{ matrix[1][1]?matrix[1][1]: '' }}
        </div>
      </div>
      <div
        class="grid-item right"
        :class="[!matrix[1][2] &&
          !playerOneWon && !playerTwoWon ?'pointer':'']"
        @click="!matrix[1][2] && !playerOneWon && !playerTwoWon &&
          clickCell(1,2)"
      >
        <div :class="[winningSelection(`${1}${2}`)?'blink':'']">
          {{ matrix[1][2]?matrix[1][2]: '' }}
        </div>
      </div>
      <div
        class="grid-item bottom left"
        :class="[!matrix[2][0] &&
          !playerOneWon && !playerTwoWon ?'pointer':'']"
        @click="!matrix[2][0] && !playerOneWon && !playerTwoWon &&
          clickCell(2,0)"
      >
        <div :class="[winningSelection(`${2}${0}`)?'blink':'']">
          {{ matrix[2][0]?matrix[2][0]: '' }}
        </div>
      </div>
      <div
        class="grid-item bottom center"
        :class="[!matrix[2][1] &&
          !playerOneWon && !playerTwoWon ?'pointer':'']"
        @click="!matrix[2][1] && !playerOneWon && !playerTwoWon &&
          clickCell(2,1)"
      >
        <div :class="[winningSelection(`${2}${1}`)?'blink':'']">
          {{ matrix[2][1]?matrix[2][1]: '' }}
        </div>
      </div>
      <div
        class="grid-item bottom right"
        :class="[!matrix[2][2] &&
          !playerOneWon && !playerTwoWon ?'pointer':'']"
        @click="!matrix[2][2] && !playerOneWon && !playerTwoWon &&
          clickCell(2,2)"
      >
        <div :class="[winningSelection(`${2}${2}`)?'blink':'']">
          {{ matrix[2][2]?matrix[2][2]: '' }}
        </div>
      </div>
    </div>
    <!-- <div class="gameContainer">
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
              !playerOneWon && !playerTwoWon ?'pointer':'']"
            @click="!matrix[rowIndex][coulumnIndex] && !playerOneWon && !playerTwoWon &&
              clickCell(rowIndex,coulumnIndex)"
          >
            {{ matrix[rowIndex][coulumnIndex]?matrix[rowIndex][coulumnIndex]:'' }}
          </td>
        </tr>
      </table>
    </div> -->
    <br>
    <button @click="resetGame()">
      Restart Game
    </button>
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
.success{
  background: greenyellow;
}
.pointer{
  cursor: pointer;
}
.container{
  width: 30%;
}
.top{
  border-top: 0px !important;
}
.bottom{
  border-bottom: 0px  !important;
}
.left{
  border-left: 0px  !important;
}
.right{
  border-right: 0px  !important;
}
.grid-container {
  display: grid;
  grid-template-columns: auto auto auto;
  width:300px;
  margin: auto;
}
.grid-item {
  border: 2px solid white;
  height: 85px;
  font-size: 60px;
  color: white;
  text-align: center;
  width:100px;
}
@-webkit-keyframes blinker {
  from {opacity: 1.0;}
  to {opacity: 0.0;}
}
.blink{
  text-decoration: blink;
  -webkit-animation-name: blinker;
  -webkit-animation-duration: 0.3s;
  -webkit-animation-iteration-count:infinite;
  -webkit-animation-timing-function:ease-in-out;
  -webkit-animation-direction: alternate;
}
.outer-container{
  color:white;
}
</style>
