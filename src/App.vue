<template>
  <div class="container mt-5">
    <h1 class="text-center">Rock Paper Scissors Love</h1>

    <div class="row justify-content-center">
      <div class="col-md-6">
        <div class="player text-center">
          <h2>Player 1</h2>
          <img v-if="player1Choice" :src="player1ChoiceImage" alt="Player 1 Choice" class="img-fluid max-size">
        </div>
      </div>
      <div class="col-md-6">
        <div class="player text-center">
          <h2>Player 2</h2>
          <img v-if="player2Choice" :src="player2ChoiceImage" alt="Player 2 Choice" class="img-fluid max-size">
        </div>
      </div>
    </div>

    <div class="row justify-content-center mt-3">
      <div class="col-md-6 text-center">
        <p class="lead">Score: {{ score }}</p>
        <button @click="startRound" class="btn btn-primary mx-2">Start Next Round</button>
        <button @click="restart" class="btn btn-danger mx-2">Restart Game</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      player1Choice: '',
      player2Choice: '',
      score: '0:0',
      choices: ['rock', 'paper', 'scissors', 'love'],
      choiceImages: {
        scissors: 'https://www.shutterstock.com/shutterstock/photos/337423679/display_1500/stock-vector-scissors-icon-337423679.jpg',
        rock: 'https://images.thdstatic.com/productImages/4f9a9c81-0742-4be3-b016-074bf6a9a076/svn/stanley-claw-hammers-stht51456-64_1000.jpg',
        paper: 'https://bookshellbindery.com/cdn/shop/products/Shortgrain-paper_Bookshell_A3_Lined_White_Cream_1_SFW_1500px.jpg?v=1602594700',
        love: 'https://wallpaperaccess.com/full/4169028.jpg'
      },
    };
  },
  computed: {
    player1ChoiceImage() {
      return this.choiceImages[this.player1Choice];
    },
    player2ChoiceImage() {
      return this.choiceImages[this.player2Choice];
    },
  },
  methods: {
    getRandomChoice(choices) {
      const randomIndex = Math.floor(Math.random() * choices.length);
      return choices[randomIndex];
    },
    startRound() {
      const player1Choice = this.getRandomChoice(this.choices);
      const player2Choice = this.getRandomChoice(this.choices);

      this.player1Choice = player1Choice;
      this.player2Choice = player2Choice;

      const winner = this.determineWinner(player1Choice, player2Choice);
      let [player1Score, player2Score] = this.score.split(':').map(Number);

      if (winner === 'player1') {
        player1Score += 1;
      } else if (winner === 'player2') {
        player2Score += 1;
      }

      this.score = `${player1Score}:${player2Score}`;
    },
    restart() {
      this.score = '0:0';
      this.player1Choice = '';
      this.player2Choice = '';
    },
    
    determineWinner(player1, player2) {
      if (player1 === player2) return 'tie';
      if (
        (player1 === 'rock' && player2 === 'scissors') ||
        (player1 === 'paper' && player2 === 'rock') ||
        (player1 === 'scissors' && player2 === 'paper') ||
        (player1 === 'love' && player2 === 'rock') ||
        (player1 === 'love' && player2 === 'paper') ||
        (player1 === 'love' && player2 === 'scissors') 
      ) {
        return 'player1';
      }
      return 'player2';
    },
  },
};
</script>

<style>
.game-container {
  display: flex;
  justify-content: space-around;
  margin: 20px;
}

.player {
  text-align: center;
}

img {
  max-width: 150px;
}

.score-container {
  text-align: center;
  margin-top: 20px;
}

button {
  margin: 10px;
}

.max-size {
  max-width: 500px;
  max-height: 500px;
  width: auto;
  height: auto;
}
</style>