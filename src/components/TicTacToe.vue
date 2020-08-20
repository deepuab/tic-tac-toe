<template>
  <div>
    <b-container> 
      <div v-if="this.playerOneWon">Player One won!!</div>
      <div v-if="this.playerTwoWon">Player Two won!!</div>
      <div v-if="draw">Draw!!</div>
  <table>
  <tr v-for="(row, rowIndex) in matrix" :key="rowIndex">
    <td v-for="(col,coulumnIndex) in row" :key="rowIndex+coulumnIndex" @click="clickCell(rowIndex,coulumnIndex,$event)" :style="[winningSelection(`${rowIndex}${coulumnIndex}`)?{'background':'greenyellow'}:'']">
      {{matrix[rowIndex][coulumnIndex]?matrix[rowIndex][coulumnIndex]:''}}
    </td>
  </tr>
</table>
<button @click="resetGame()">Reset</button>
    </b-container>
  </div>
</template>

<script>
export default {
  name: 'TicTacToe',
     data :function() {
return {
              matrix:[
              [null, null, null],
              [null, null, null],
              [null, null, null]
            ],
            previousSelection:null ,
            playerOneSelectedCells:[], 
            winningData:[],
            playerOneWon:false,
            playerTwoSelectedCells:[],  
            playerTwoWon:false,
            winningCells:[
              ['00','01','02'],
              ['00','11','22'],
              ['00','10','20'],
              ['01','11','21'],
              ['02','12','22'],
              ['02','11','20'],
              ['10','11','12'],
              ['20','21','22'],
            ]    
        };
},
computed:{
  playerOneWin(){
    let match=false;
    match=this.winningCells.some(com=>{
    const test= com.every(val=>{     
        return this.playerOneSelectedCells.includes(val);
      });
      if(test){
        return true;
      }else{
        return false;
      }
   
    });
    return match;
  },
  playerTwoWin(){
    let match=false;
    match=this.winningCells.some(com=>{
    const test= com.every(val=>{     
        return this.playerTwoSelectedCells.includes(val);
      });
      if(test){
        return true;
      }else{
        return false;
      }
   
    });
    return match;
  },
  draw(){
    return this.playerOneSelectedCells.length==5 && !this.playerOneWon && !this.playerTwoWon
  },
},
  methods:{
    clickCell(row,col,event){
      event.target.style.pointerEvents = 'none';
      const newRow = this.matrix[row].slice(0);
      if(!this.previousSelection || this.previousSelection==='O'){
        this.previousSelection='X';
        this.playerOneSelectedCells.push(`${row}${col}`);
      }else{
        this.previousSelection='O';
        this.playerTwoSelectedCells.push(`${row}${col}`);
      }
      newRow[col] = this.previousSelection;      
      this.$set(this.matrix, row, newRow);
      this.checkGameStatus();
    },
    checkGameStatus(){
    let match=false;
    let match1=false;
    // let winningData=[];
    match=this.winningCells.some(com=>{
    const test= com.every(val=>{     
        return this.playerOneSelectedCells.includes(val);
      });
      if(test){
        this.winningData=com;
        return true;
      }else{
        return false;
      }
   
    });

    match1=this.winningCells.some(com=>{
    const test= com.every(val=>{     
        return this.playerTwoSelectedCells.includes(val);
      });
      if(test){
        this.winningData=com;
        return true;
      }else{
        return false;
      }
   
    });
    this.playerOneWon=match;
    this.playerTwoWon=match1;

    },
    resetGame(){
      this.matrix=[
              [null, null, null],
              [null, null, null],
              [null, null, null]
            ];
      this.previousSelection=null;
      this.playerOneSelectedCells=[]; 
      this.playerTwoSelectedCells=[];  
      this.playerOneWon=false;
      this.playerTwoWon=false; 
      this.winningData=[];   

    },
   winningSelection(index){
     return this.winningData.includes(index)
  }
  }
}
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
  min-height: 50px;
  height: 50px;
}
.success{
  background: greenyellow;
}
</style>
