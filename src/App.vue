<template>
  <div id="app">
    <h1 class="title">Memory Game</h1>
    <div class="game">
      <div class="counter">
        <h1>{{ counter }} s.</h1>
      </div>
      <div class="buttons">
        <button class="btn" @click="onStartGame" :disabled="isGameStarted">Start new game</button>
        <button class="btn" @click="onResetGame" :disabled="!isGameStarted">Reset game</button>
      </div>
      <div class="result" v-if="isGameOver">
        <p v-if="areAllFound" class="win">YOU ARE WINNER!</p>
        <p v-else class="fail">YOU LOST THE GAME!</p>
      </div>
      <section class="memory-game">
        <div
          v-for="(card, index) in shuffledCards"
          :key="index"
          class="memory-card"
          :class="{ flip: card.visible, disabled: card.matched }"
          :data-framework="card.name"
          @click="showCard(card, index)"
        >
          <img class="front-face" :src="card.image" :alt="card.name" />
          <img
            class="back-face"
            src="./assets/img/js-badge.svg"
            alt="JS Badge"
          />
        </div>
      </section>
    </div>
  </div>
</template>

<script>
let timeSeconds = 60;
let interval;
export default {
  name: "App",
  data() {
    return {
      counter: timeSeconds,
      isGameStarted: false,
      isGameOver: false,
      foundPairs: 0,
      selectedCards: [],
      shuffledCards: [],
      cards: [
        {
          name: "react",
          image: require("./assets/img/react.svg"),
          visible: false,
          matched: false,
        },
        {
          name: "backbone",
          image: require("./assets/img/backbone.svg"),
          visible: false,
          matched: false,
        },
        {
          name: "ember",
          image: require("./assets/img/ember.svg"),
          visible: false,
          matched: false,
        },
        {
          name: "angular",
          image: require("./assets/img/angular.svg"),
          visible: false,
          matched: false,
        },
        {
          name: "vue",
          image: require("./assets/img/vue.svg"),
          visible: false,
          matched: false,
        },
        {
          name: "aurelia",
          image: require("./assets/img/aurelia.svg"),
          visible: false,
          matched: false,
        },
      ],
    };
  },
  watch: {
    selectedCards(newValue) {
      if (newValue.length == 2) {
        const [firstIndex, secondIndex] = newValue;
        const firstCard = this.shuffledCards[firstIndex];
        const secondCard = this.shuffledCards[secondIndex];

        if (firstCard.name === secondCard.name && firstIndex !== secondIndex) {
          firstCard.matched = true;
          secondCard.matched = true;
          this.foundPairs += 1;
        } else {
          setTimeout(() => {
            firstCard.visible = false;
            secondCard.visible = false;
          }, 1000);
        }
        this.selectedCards = [];
      }
    },
    counter(newValue) {
      if (newValue === 0) {
        this.clearTimer();
        this.handleGameEnd();
   
      }
    },
    areAllFound(newValue, oldValue) {
      if (newValue !== oldValue && newValue) {
        this.handleFinishedGame();
      }
    },
  },
  computed: {
    areAllFound() {   
      console.log(this.foundPairs);   
      return this.cards.length === this.foundPairs;
    },
  },

  methods: {
    randomize(arr, n) {
      for (let i = n - 1; i > 0; i--) {
        let j = Math.floor(Math.random() * (i + 1));
        [arr[i], arr[j]] = [arr[j], arr[i]];
      }
    },
    generateShuffledCards() {
      const cards = [
        ...JSON.parse(JSON.stringify(this.cards)),
        ...JSON.parse(JSON.stringify(this.cards)),
      ];
      this.randomize(cards, cards.length);
      this.shuffledCards = cards;
    },
    showCard(card, index) {
      card.visible = true;
      this.selectedCards.push(index);
    },
    startCounter() {
      this.counter = timeSeconds;
      interval = setInterval(() => {
        this.counter -= 1;
        if(this.counter==0) {
          this.counter=0;
        }
      }, 1000);
    },
    onStartGame() {
      this.isGameStarted = true;
      this.resetState();      
      this.generateShuffledCards();
      this.startCounter();
    },
    clearTimer() {
      clearInterval(interval);
    },
    handleGameEnd() {
      this.isGameOver = true;
      this.isGameStarted = false;
      if(!this.areAllFound) {
        this.shuffledCards.forEach((card) => (card.matched = true));
      }
    },
    resetState() {
      this.counter = timeSeconds;
      this.foundPairs = 0;
      this.shuffledCards = [];
      this.isGameOver = false;

    },
    onResetGame () {
      this.clearTimer();
      this.resetState();
      this.isGameStarted = false;

    },
    handleFinishedGame() {
      this.clearTimer();
      this.handleGameEnd();
    }
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
body {
  height: 100vh;
  display: flex;
  background-image: linear-gradient(120deg, #d4fc79 0%, #96e6a1 100%);
}
.title {
  text-align: center;
  margin-bottom: 30px;
  text-transform: uppercase;
}
#app {
  width: 100%;
  padding: 7%;
}
.buttons {
  width: 640px;
  margin: 0 auto 30px auto;
}
.btn {
  padding: 10px;
  border-radius: 10px;
}
.result{
  width: 600px;
  margin: 30px auto;
  padding: 10px;
}
.win {
  background-color: green;
  border: 1px solid darkgreen;
  border-radius: 10px;
  text-align: center;
  padding: 10px;
  color: white;
  letter-spacing: 1px;
}
.fail {
  background-color: red;
  border: 1px solid darkred;
  border-radius: 10px;
  text-align: center;
  padding: 10px;
  color: white;
  letter-spacing: 1px;
}

.memory-game {
  width: 640px;
  height: 640px;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
  perspective: 1000px;
}
.memory-card {
  width: calc(25% - 10px);
  height: calc(33.333% - 10px);
  margin: 5px;
  position: relative;
  transform: scale(1);
  transform-style: preserve-3d;
  transition: transform 0.5s;
  box-shadow: 1px 1px 1px rgba(0, 0, 0, 0.3);
}

.disabled {
  pointer-events: none;
}

.memory-card:active {
  transform: scale(0.97);
  transition: transform 0.2s;
}

.memory-card.flip {
  transform: rotateY(180deg);
}

.front-face,
.back-face {
  width: 100%;
  height: 100%;
  padding: 20px;
  position: absolute;
  border-radius: 5px;
  background: #fff29e;
  backface-visibility: hidden;
}

.front-face {
  transform: rotateY(180deg);
}

.counter {
  position: absolute;
  top: 145px;
  right: 240px;
}
</style>
