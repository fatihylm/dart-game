<template>
  <div class="dart-scorer">
    <div class="scoreboard">
      <div class="player" v-for="(player, index) in players" :key="index">
        <h1>{{ player.name }}</h1>
        <div class="score">{{ player.score }}</div>
      </div>
    </div>
    <div class="controls">
      <div class="player" v-for="(player, index) in players" :key="index">
        <div class="buttons">
          <div class="button" @click="subtractScore(player, 1)">1</div>
          <div class="button" @click="subtractScore(player, 2)">2</div>
          <div class="button" @click="subtractScore(player, 3)">3</div>
          <div class="button" @click="subtractScore(player, 4)">4</div>
        </div>
        <div class="buttons">
          <div class="button" @click="subtractScore(player, 5)">5</div>
          <div class="button" @click="subtractScore(player, 6)">6</div>
          <div class="button" @click="subtractScore(player, 7)">7</div>
          <div class="button" @click="subtractScore(player, 8)">8</div>
        </div>
        <div class="buttons">
          <div class="button" @click="subtractScore(player, 9)">9</div>
          <div class="button" @click="subtractScore(player, 10)">10</div>
          <div class="button" @click="subtractScore(player, 11)">11</div>
          <div class="button" @click="subtractScore(player, 12)">12</div>
          <div class="button" @click="subtractScore(player, 0)">0</div>
        </div>
        <div class="buttons">
          <div class="button" @click="subtractScore(player, 13)">13</div>
          <div class="button" @click="subtractScore(player, 14)">14</div>
          <div class="button" @click="subtractScore(player, 15)">15</div>
          <div class="button" @click="subtractScore(player, 16)">16</div>
          <div class="button" @click="subtractScore(player, 25)">25</div>
        </div>
        <div class="buttons">
          <div class="button" @click="subtractScore(player, 17)">17</div>
          <div class="button" @click="subtractScore(player, 18)">18</div>
          <div class="button" @click="subtractScore(player, 19)">19</div>
          <div class="button" @click="subtractScore(player, 20)">20</div>
          <div class="button" @click="subtractScore(player, 50)">50</div>
        </div>
        <div class="buttons">
          <div class="button" @click="subtractScore(player, 0)">Double</div>
          <div class="button" @click="subtractScore(player, 0)">Triple</div>
        </div>

        <div v-if="player.winner" class="winner-label">Winner!</div>
      </div>
    </div>
    <div class="reset">
      <div class="button" @click="resetGame()">Reset</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DartScorer",
  data() {
    return {
      players: [
        {
          name: "Player 1",
          score: 501,
          buttonPresses: 0,
          buttonsDisabled: false,
          isCurrentPlayer: true,
        },
        {
          name: "Player 2",
          score: 501,
          buttonPresses: 0,
          buttonsDisabled: true,
          isCurrentPlayer: false,
        },
      ],
      currentPlayerIndex: 0,
    };
  },
  methods: {
    subtractScore(player, points) {
      if (player.buttonsDisabled) return;
      let previousScore = player.score;
      player.score -= points;
      player.buttonPresses += 1;
      if (player.buttonPresses >= 3) {
        this.disableButtons(player);
        this.changePlayer();
      }
      if (player.score > 501 || player.score < 0) {
        player.score = previousScore;
      }
      if (player.score == 0) {
        player.winner = true;
      }
    },
    changePlayer() {
      this.currentPlayerIndex = (this.currentPlayerIndex + 1) % 2;
      let current = this.players[this.currentPlayerIndex];
      current.buttonPresses = 0;
      current.buttonsDisabled = false;
      let other = this.players[(this.currentPlayerIndex + 1) % 2];
      other.buttonsDisabled = true;
    },
    disableButtons(player) {
      player.buttonsDisabled = true;
      if (player.subtractScore <= 3) {
        player.buttons.forEach((button) => {
          button.disabled = true;
          button.style.opacity = 0.1;
        });
      }
    },
    resetGame() {
      this.players.forEach((player) => (player.score = 501));
      this.players.forEach((player) => (player.winner = false));
    },
  },
};
</script>

<style>
.winner-label {
  color: black;
  font-size: 60px;
}
.dart-scorer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
.scoreboard {
  display: flex;
  justify-content: space-around;
  width: 100%;
  margin-bottom: 16px;
}
.player {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.score {
  font-size: 48px;
  font-weight: bold;
  margin-top: 16px;
}
.controls {
  display: flex;
  justify-content: space-around;
  width: 100%;
}
.buttons {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
.button {
  padding: 20px;
  margin: 8px;
  font-size: 24px;
  border: none;
  border-radius: 8px;
  color: white;
  cursor: pointer;
}

.button:nth-child(1),
.button:nth-child(4) {
  background-color: red;
}
.button:nth-child(2),
.button:nth-child(5) {
  background-color: blue;
}
.button:nth-child(3),
.button:nth-child(6) {
  background-color: green;
}
.button:nth-child(7) {
  background-color: orange;
}
.button:nth-child(8) {
  background-color: purple;
}
.reset {
  display: flex;
  justify-content: center;
}
</style> 
