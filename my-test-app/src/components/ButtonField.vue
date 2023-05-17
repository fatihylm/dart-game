<template>
  <div class="dart-scorer">
    <p>
      DARTSCORER<br />
      PLAYER 1 ALWAYS STARTS
    </p>
    <div class="scoreboard">
      <div class="player" v-for="(player, index) in players" :key="index">
        <h1>{{ player.name }}</h1>
        <div class="score">{{ player.score }}</div>
        <div class="throws">
          <h2>Throws: {{ player.throws }}</h2>
        </div>
      </div>
    </div>
    <div class="controls">
      <div class="player" v-for="(player, index) in players" :key="index">
        <div class="buttons" v-for="row in buttons" :key="row">
          <div
            class="button"
            v-for="button in row"
            :key="button"
            @click="subtractScore(player, button)">
            {{ button }}
          </div>
        </div>
        <div v-if="player.winner" class="winner-label">Winner!</div>
      </div>
    </div>
    <div>
      <div class="button" @click="doubleButtons">
        {{ double ? "Double:ON" : "Double:OFF" }}
      </div>
      <div class="button" @click="tripleButtons">
        {{ triple ? "Triple:ON" : "Triple:OFF" }}
      </div>
    </div>
    <div class="reset">
      <div class="button" @click="resetGame()">RESTART</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "DartScorer",
  data() {
    return {
      buttons: [
        [1, 2, 3, 4],
        [5, 6, 7, 8],
        [9, 10, 11, 12, 0],
        [13, 14, 15, 16, 25],
        [17, 18, 19, 20, 50],
      ],
      double: false,
      triple: false,
      players: [
        {
          name: "Player 1",
          score: 501,
          buttonPresses: 0,
          buttonsDisabled: false,
          isCurrentPlayer: true,
          throws: 0,
        },
        {
          name: "Player 2",
          score: 501,
          buttonPresses: 0,
          buttonsDisabled: true,
          isCurrentPlayer: false,
          throws: 0,
        },
      ],
      currentPlayerIndex: 0,
      showTurnOver: false,
    };
  },
  methods: {
    subtractScore(player, points) {
      if (player.buttonsDisabled) return;
      let previousScore = player.score;
      player.score -= points;
      player.throws++;
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
      if (player.throws === 3) {
        player.throws = 0;
        alert(`${player.name} has made 3 button presses. NEXT PLAYER BEGINS`);
      }
    },
    doubleButtons() {
      this.double = !this.double;
      if (this.double == true) {
        this.buttons = [
          [2, 4, 6, 8],
          [10, 12, 14, 16],
          [18, 20, 22, 24, 0],
          [26, 28, 30, 32, 25],
          [34, 36, 38, 40, 50],
        ];
      }

      if (this.double == false) {
        this.buttons = [
          [1, 2, 3, 4],
          [5, 6, 7, 8],
          [9, 10, 11, 12, 0],
          [13, 14, 15, 16, 25],
          [17, 18, 19, 20, 50],
        ];
      }
      if (this.double && this.triple) {
        this.triple = false;
      }
    },
    tripleButtons() {
      this.triple = !this.triple;
      if (this.triple == true) {
        this.buttons = [
          [3, 6, 9, 12],
          [15, 18, 21, 24],
          [27, 30, 33, 36, 0],
          [39, 42, 45, 48, 25],
          [51, 54, 57, 60, 50],
        ];
      }

      if (this.triple == false) {
        this.buttons = [
          [1, 2, 3, 4],
          [5, 6, 7, 8],
          [9, 10, 11, 12, 0],
          [13, 14, 15, 16, 25],
          [17, 18, 19, 20, 50],
        ];
      }
      if (this.triple && this.double) {
        this.double = false;
      }
    },
    buttonPressed(index) {
      let button = this.buttons[index];
      if (this.lastDoublePressed) {
        button.value *= 2;
        button.doubled = true;
        this.lastDoublePressed = false;
      } else {
        button.doubled = false;
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
        return;
      }
    },

    resetGame() {
      this.currentPlayerIndex = 0;
      this.players.forEach((player) => {
        player.score = 0;
        player.buttonPresses = 0;
        player.buttonsDisabled = false;
        player.throws = 0;
      });
      this.players.forEach((player) => (player.score = 501));
      this.players.forEach((player) => (player.winner = false));
      this.triple = false;
      this.double = false;
      this.buttons = [
        [1, 2, 3, 4],
        [5, 6, 7, 8],
        [9, 10, 11, 12, 0],
        [13, 14, 15, 16, 25],
        [17, 18, 19, 20, 50],
      ];
    },
  },
};
</script>

<style>
p {
  display: flex;
  justify-content: center;
  font-size: 40px;
  margin-bottom: 5px;
}
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
.button:active {
  background-color: aqua;
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
.TESTBUTTON {
  padding: 20px;
  margin: 8px;
  font-size: 24px;
  color: white;
  cursor: pointer;
  background-color: black;
}
.button:nth-child(1),
.button:nth-child(4) {
  background-color: black;
}
.button:nth-child(2),
.button:nth-child(5) {
  background-color: black;
}
.button:nth-child(3),
.button:nth-child(6) {
  background-color: black;
}
.button:nth-child(7) {
  background-color: black;
}
.button:nth-child(8) {
  background-color: black;
}
.reset {
  display: flex;
  justify-content: center;
}
</style> 
