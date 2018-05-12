<template> 
  <table class="center">
  	<div v-if="newGame"> 
  	<header>
        <span v-if="playerOneTurn && !gameOver" class="playerCard">
          Your turn
        </span>
        <button v-if="moves == 9 || gameOver" @click="restart" class="restart playerCard">
          Game Over. Restart?
        </button>
        <span v-if="!playerOneTurn && !gameOver" class="playerCard">
          Computer's turn
        </span>
    </header>
    <tr class="grid">
      <Cell name="0" :markedStatus="cells[0]"></Cell>
      <Cell name="1" :markedStatus="cells[1]"></Cell>
      <Cell name="2" :markedStatus="cells[2]"></Cell>
    </tr>
    <tr class="grid">
      <Cell name="3" :markedStatus="cells[3]"></Cell>
      <Cell name="4" :markedStatus="cells[4]"></Cell>
      <Cell name="5" :markedStatus="cells[5]"></Cell>
    </tr>
    <tr class="grid">
      <Cell name="6" :markedStatus="cells[6]"></Cell>
      <Cell name="7" :markedStatus="cells[7]"></Cell>
      <Cell name="8" :markedStatus="cells[8]"></Cell>
    </tr>
    <tr class="score">
      <span class="score">Scoreboard</span>
    </tr>
    <footer>  	
        <span>
          You have {{ playerWins }} win<span v-if="playerWins!=1">s</span>.
        </span>
        
        <span>
          Computer has {{ computerWins }} win<span v-if="computerWins!=1">s</span>.
        </span>
    </footer>
    </div>
    <div class="newGame" v-else>
      <h1>Let's play Tic Tac Toe!</h1>
      <h2>
        Would you like to be X or O?
      </h2>
      <div class="markSelection">
      	<button @click="setPlayerMark('X')">X</button>
        <button @click="setPlayerMark('O')">O</button>
      </div>
    </div>
  </table>
  
    
</template>

<script>
import Cell from './Cell'

export default {
  name: 'Board',
  components: {
    Cell
  },
  data () {
    return {
      moves: 0,
      cells: [
      1, 2, 3, 4, 5, 6, 7, 8, 9],
      gameStatus: '',
      newGame: false,
      playerWins: 0,
      computerWins: 0,
      activePlayer: '',
      playerMark: '',
      computerMark: '',
      winningCombinations: [
        [0, 1, 2],
    	[0, 4, 8],
    	[0, 3, 6],
    	[1, 4, 7],
    	[2, 5, 8],
    	[2, 4, 6],
    	[3, 4, 5],
    	[6, 7, 8],
      ]
    }
  },
  created() {
  	Event.$on('mark', (cellNumber) => {
	  this.cells[cellNumber] = this.activePlayer;
      this.moves++;
      this.gameStatus = this.changeGameStatus();
      this.changePlayer();
      if(this.moves % 2 === 1 && !this.gameOver) {
      	this.computerMove();
      }
      
  	})
  },
  methods: {
  	setPlayerMark(mark) {
      this.playerMark = mark;
      this.activePlayer = mark;
      if(mark === 'X'){
      	this.computerMark = 'O';
      } else { this.computerMark = 'X' }
      this.newGame = true;
    },
    computerMove() {
      this.moves++;
      this.gameStatus = this.changeGameStatus();
      let moveAttempts = [];
      setTimeout( () => {
        for(let i = 0; i < this.cells.length; i++) {
      	moveAttempts.push(Math.floor(Math.random() * Math.floor(9)));
      	if (typeof this.cells[moveAttempts[i]] === "number"){
      		this.cells[moveAttempts[i]] = this.computerMark;
      		this.changePlayer();
      		return;
      	}
      }
      }, 1000) 
    },
    changePlayer() {
      this.activePlayer = this.nonActivePlayer;
    },
    changeGameStatus() {
      if (this.checkForWin()) {
      	return true;
      } else if (this.moves === 9) {
      	return 'draw';
      }
      return 'turn';
    },
    checkForWin() {
    	for (let combo of this.winningCombinations) {
    		if(isNaN(this.cells[combo[0]]) && this.cells[combo[0]] ==
    			this.cells[combo[1]] &&
    			this.cells[combo[0]] ==
    			this.cells[combo[2]]){
    			if(this.cells[combo[0]] == 
    				this.playerMark){
                    this.playerWins++;
    			} else { this.computerWins++; }
    			return true;
    		}
    	}
    	return false;
    },
    restart() {
    	this.gameStatus = '';
    	this.moves = 0;
    	this.cells = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    	this.activePlayer = this.playerMark;
    }
  },
  computed: {
  	nonActivePlayer() {
  		if(this.activePlayer === 'X'){
  	      return 'O';
  		}
  		return 'X';
  	},
    playerOneTurn() {
      return this.playerMark == this.activePlayer;
    },
    gameOver() {
    	return this.gameStatus === true;
    }
  }
}
</script>

<style>
header, footer, .playerCard, .markSelection, .score,
.markSelection > button {
  font-family: 'Indie Flower';
  display: flex;
  justify-content: space-between;
  border-radius: 50px;
  font-size: 2em;
  background-color: tan;
  margin: 0 auto;
  color: #CC3300;
}
.markSelection > button {
	background-color: white;
}
.playerCard {
  text-align: center;
  font-size: 1.5em;
  padding: 10px;
  margin-bottom: 10px;
}
.restart {
  border-radius: 25px;
  border: 3px solid #663300;
}
.score {
  font-size: 1.5em;
  font-weight: bold;
  text-decoration: underline;
  margin: 0 auto;
  text-align: center;
}
.newGame {
  color: #CC3300;
  font-size: 1.1em;
  font-family: 'Indie Flower';
  padding: 100px;
}
.grid {
  display: flex;
  justify-content: space-around;
  margin: 0 auto;
  padding: 0;
  
}
.center {
  margin: 50px auto;
  background-color: tan;
  padding: 10px;
  border-radius: 25px;
  border: 10px solid #663300;

}
</style>
