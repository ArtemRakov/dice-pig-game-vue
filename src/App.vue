<template>
  <div id='app'>
     <div class="wrapper clearfix">
            <div class="player-0-panel" :class="{active: activePlayer === 0}">
                <div class="player-name" id="name-0">{{ scores.player0.winner }}</div>
                <div class="player-score" id="score-0">{{ scores.player0.totalScore }}</div>
                <div class="player-current-box">
                    <div class="player-current-label">Current</div>
                    <div class="player-current-score" id="current-0">{{ scores.player0.currentRoll }}</div>
                </div>
            </div>
            
            <div class="player-1-panel" :class="{active: activePlayer === 1}">
                <div class="player-name" id="name-1">{{ scores.player1.winner }}</div>
                <div class="player-score" id="score-1">{{ scores.player1.totalScore }}</div>
                <div class="player-current-box">
                    <div class="player-current-label">Current</div>
                    <div class="player-current-score" id="current-1">{{ scores.player1.currentRoll }}</div>
                </div>
            </div>
            
            <button class="btn-new" @click="newGame"><i class="ion-ios-plus-outline"></i>New game</button>
            <span class="label"> Select final score </span>
            <input type="text" name='input' v-model="finalScore" class="input-score">
            <button class="btn-roll" v-if="!gameOver" @click="rollDice"><i class="ion-ios-loop"></i>Roll dice</button>
            <button class="btn-hold" v-if="!gameOver" @click="hold"><i class="ion-ios-download-outline"></i>Hold</button>
            
            <img v-if="dice" :src="require(`./assets/dice-${this.dice}.png`)" alt="Dice" class="dice">
        </div>
  </div>
</template>

<script>
export default {
  data() {
   return {
     scores: {
       player0: {
         currentRoll: 0,
         totalScore: 0,
         winner: "Player 1",
       },
       player1: {
         currentRoll: 0,
         totalScore: 0,
         winner: "Player 2"
       }
     },
     activePlayer: 0,
     dice: false,
     gameOver: false,
     finalScore: 50
   } 
  },
  methods: {
    rollDice() {
      const prevoiusDice = this.dice
      const dice = Math.floor(Math.random() * 6 + 1)
      this.dice = dice
      if (prevoiusDice === 6 && dice === 6 ) {
        this.scores['player' + this.activePlayer].totalScore = 0;
        this.scores['player' + this.activePlayer].currentRoll = 0;
        this.activePlayer === 0 ? this.activePlayer = 1 : this.activePlayer = 0 
      }
      else if (dice !== 1) {
        this.scores['player' + this.activePlayer].currentRoll += dice
      }
      else {
        this.scores['player' + this.activePlayer].currentRoll = 0
        this.activePlayer === 0 ? this.activePlayer = 1 : this.activePlayer = 0 
      }
    },
    hold() {
      const score = this.scores['player' + this.activePlayer].currentRoll
      this.scores['player' + this.activePlayer].currentRoll = 0
      this.scores['player' + this.activePlayer].totalScore += score
      this.finalScore ? this.finalScore : this.finalScore = 50
      if (this.scores['player' + this.activePlayer].totalScore >= this.finalScore) {
        this.scores['player' + this.activePlayer].winner = "Winner!"
        this.dice = false
        this.gameOver = true
        return
      }
      this.activePlayer === 0 ? this.activePlayer = 1 : this.activePlayer = 0 
    },
    newGame() {
      this.scores = {
       player0: {
         currentRoll: 0,
         totalScore: 0,
         winner: "Player 1",
       },
       player1: {
         currentRoll: 0,
         totalScore: 0,
         winner: "Player 2"
       }
     }
     this.activePlayer = 0
     this.dice = false
     this.gameOver = false
    }
  }
};
</script>

<style>
/**********************************************
*** GENERAL
**********************************************/

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    
}

.clearfix::after {
    content: "";
    display: table;
    clear: both;
}

body {
    background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url(./assets/back.jpg);
    background-size: cover;
    background-position: center;
    font-family: Lato;
    font-weight: 300;
    position: relative;
    height: 100vh;
    color: #555;
}

.wrapper {
    width: 1000px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: #fff;
    box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
    overflow: hidden;
}

.player-0-panel,
.player-1-panel {
    width: 50%;
    float: left;
    height: 600px;
    padding: 100px;
}



/**********************************************
*** PLAYERS
**********************************************/

.player-name {
    font-size: 40px;
    text-align: center;
    text-transform: uppercase;
    letter-spacing: 2px;
    font-weight: 100;
    margin-top: 20px;
    margin-bottom: 10px;
    position: relative;
}

.player-score {
    text-align: center;
    font-size: 80px;
    font-weight: 100;
    color: #EB4D4D;
    margin-bottom: 130px;
}

.active { background-color: #f7f7f7; }
.active .player-name { font-weight: 300; }

.active .player-name::after {
    content: "\2022";
    font-size: 47px;
    position: absolute;
    color: #EB4D4D;
    top: -7px;
    right: 10px;
    
}

.player-current-box {
    background-color: #EB4D4D;
    color: #fff;
    width: 40%;
    margin: 0 auto;
    padding: 12px;
    text-align: center;
}

.player-current-label {
    text-transform: uppercase;
    margin-bottom: 10px;
    font-size: 12px;
    color: #222;
}

.player-current-score {
    font-size: 30px;
}

button {
    position: absolute;
    width: 200px;
    left: 50%;
    transform: translateX(-50%);
    color: #555;
    background: none;
    border: none;
    font-family: Lato;
    font-size: 20px;
    text-transform: uppercase;
    cursor: pointer;
    font-weight: 300;
    transition: background-color 0.3s, color 0.3s;
}

button:hover { font-weight: 600; }
button:hover i { margin-right: 20px; }

button:focus {
    outline: none;
}

i {
    color: #EB4D4D;
    display: inline-block;
    margin-right: 15px;
    font-size: 32px;
    line-height: 1;
    vertical-align: text-top;
    margin-top: -4px;
    transition: margin 0.3s;
}

.btn-new { top: 45px;}
.btn-roll { top: 403px;}
.btn-hold { top: 467px;}
.input-score { 
  top: 550px;
  position: absolute; 
  left: 50%;
  transform: translateX(-50%);
  font-family: Lato;
  font-size: 20px;
  text-transform: uppercase;
  background-color: transparent;
  border: none;
  text-align: center;
  border-bottom: 1px solid #555;
  }
  .input-score:focus {
    outline: none;    
  }

  .label {
    top: 520px;
    position: absolute; 
    left: 50%;
    transform: translateX(-50%);
    font-family: Lato;
    font-size: 20px;
    text-transform: uppercase;
    background-color: transparent;
    text-align: center;
  }
.dice {
    position: absolute;
    left: 50%;
    top: 178px;
    transform: translateX(-50%);
    height: 100px;
    box-shadow: 0px 10px 60px rgba(0, 0, 0, 0.10);
}

.winner { background-color: #f7f7f7; }
.winner .player-name { font-weight: 300; color: #EB4D4D; }

</style>
