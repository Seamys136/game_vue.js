<template>
  <div id="app">
    <h1>Охота на мух!</h1>
    <div class="info">
      <p>Осталось времени: {{ timeRemaining }} сек.</p>
      <p>Поймано мух: {{ score }}</p>
      <p v-if="timeRemaining <= 10" class="warning">Осталось мало времени!</p> 
    </div>
    <div class="game-container">
      <div v-if="!isGameOver">
        <div v-for="(flyIndex) in numberOfFlies" :key="flyIndex">
          <FlyGame    
            :flyImg="flyImage"
            @catch="catchFly"
          />
        </div>
      </div>
    </div>
    <div class="finish">
      <button class="btn" v-if="isGameOver" @click="startGame">Снова в бой!</button>
      <p v-if="isGameOver">Предыдущий результат: {{ highScore }}</p>
      <p v-if="newRecord" class="new-record-message">Установлен новый рекорд!</p>
      <p v-if="isGameOver" class="game-over-message">Игра окончена!</p>
    </div>
  </div>
</template>

<script>
import FlyGame from './components/FlyGame.vue';

export default {
  components: {
    FlyGame,
  },
  data() {
    return {
      flyImage: require('@/assets/muha.png'),
      score: 0,
      isGameOver: false,
      timeRemaining: 30,
      highScore: 0,
      newRecord: false,
      numberOfFlies: 5, 
    };
  },
  methods: {
    catchFly() {
      this.score++;
      this.numberOfFlies++; 
      this.updateHighScore();
    },
    updateHighScore() {
      if (this.score > this.highScore) {
        this.highScore = this.score;
        this.newRecord = true;
        localStorage.setItem('highScore', this.highScore);
      } else {
        this.newRecord = false;
      }
    },
    startGame() {
      this.score = 0;
      this.timeRemaining = 30;
      this.isGameOver = false;
      this.numberOfFlies = 7; 
      this.newRecord = false;
      this.startTimer();
    },
    startTimer() {
      const timer = setInterval(() => {
        if (this.timeRemaining > 0) {
          this.timeRemaining--;
        } else {
          clearInterval(timer);
          this.isGameOver = true; 
          this.loadHighScore(); 
        }
      }, 1000);
    },
    loadHighScore() {
      const savedHighScore = localStorage.getItem('highScore');
      if (savedHighScore !== null) {
        this.highScore = parseInt(savedHighScore);
      }
    },
  },
  mounted() {
    this.loadHighScore();
    this.startGame();
  }
};
</script>

<style>
#app {
  text-align: center;
  background: url('/src/assets/background.jpg') no-repeat center center fixed;
  background-size: cover;
  font-family: 'Roboto', Helvetica, sans-serif;
  overflow: hidden;
}

.warning {
  color: red; 
  font-weight: bold; 
}

h1 {
  margin-bottom: 20px;
}

.info {
  width: 290px;
  box-shadow: 4px 2px 4px 0px #0000001a, -4px 0px 4px 0px #0000001a;
  border-radius: 10px;
  height: 129px;
  padding: 28px 25px 25px 25px;
  box-sizing: border-box;
  background-color: #6ff03c;
  margin-left: 705px;
}

.game-container {
  position: relative; 
  width: 100vw; 
  height: 400px;
  overflow: hidden; 
  margin: 0 auto; 
}

button {
  margin-top: 20px;
  padding: 10px 20px;
  font-size: 1.2em;
  transition: background-color 0.3s, transform 0.3s;
}

button:hover {
  background-color: #1D8B00;
  transform: scale(1.05);
}

div {
  cursor: url('@/assets/muhobojka.png'), auto; 
}

.game-over-message {
  font-size: 2em;
  color: red;
  margin-top: 20px;
}

.new-record-message {
  font-size: 1.5em;
  color: green;
  margin-top: 20px;
}

.btn {
  background: #2CB708;
  border-radius: 10px;
  font-size: 18px;
  padding: 11px;
}
</style>