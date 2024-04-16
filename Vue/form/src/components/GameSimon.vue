<template>
  <div>
    <div class="simon">
      <div
          v-for="(color, index) in colors"
          :key="index"
          :style="{ backgroundColor: color }"
          @click="handleColorClick(index)"
          :class="{ active: activeIndex === index }"
      ></div>
    </div>
    <button @click="startGame" :disabled="gameStarted">Начать игру</button>
    <p v-if="gameOver">Игра окончена. Вы набрали {{ score }} очков!</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      colors: ['red', 'green', 'blue', 'yellow'],
      sequence: [],
      playerSequence: [],
      activeIndex: null,
      gameStarted: false,
      gameOver: false,
      score: 0,
      round: 1,
    };
  },
  methods: {
    startGame() {
      this.gameStarted = true;
      this.nextRound();
    },
    nextRound() {
      this.sequence = [];
      this.playerSequence = [];
      this.activeIndex = null;
      for (let i = 0; i < this.round; i++) {
        this.sequence.push(Math.floor(Math.random() * 4));
      }
      this.playSequence();
    },
    playSequence() {
      let i = 0;
      const interval = setInterval(() => {
        this.activeIndex = this.sequence[i];
        setTimeout(() => {
          this.activeIndex = null;
        }, 500);
        i++;
        if (i >= this.sequence.length) {
          clearInterval(interval);
        }
      }, 1000);
    },
    handleColorClick(index) {
      if (!this.gameStarted || this.gameOver) return;
      this.playerSequence.push(index);
      this.activeIndex = index;
      setTimeout(() => {
        this.activeIndex = null;
        if (this.playerSequence.length === this.sequence.length) {
          this.checkSequence();
        }
      }, 500);
    },
    checkSequence() {
      for (let i = 0; i < this.playerSequence.length; i++) {
        if (this.playerSequence[i] !== this.sequence[i]) {
          this.endGame();
          return;
        }
      }
      this.score++;
      this.round++;
      this.nextRound();
    },
    endGame() {
      this.gameOver = true;
    },
  },
};
</script>

<style scoped>
.simon {
  display: grid;
  grid-template-columns: repeat(10, 1fr);
  grid-template-rows: repeat(2, 1fr);
  gap: 10px;
}

.simon div {
  width: 100px;
  height: 100px;
  cursor: pointer;
}

.simon div.active {
  opacity: 0.5;
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 16px;
}
</style>
