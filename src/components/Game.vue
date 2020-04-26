<template>
  <div class="game">
    <div v-show="startScreen">
      <h1 class="title">BINGO!</h1>
      <h2>Are you...</h2>
      <button @click="showCallerScreen">The bingo caller</button>
      <button @click="createCard">A bingo player</button>
    </div>
    <div class="game-in-play" v-show="!startScreen">
    <div v-show="player" class="card-wrapper"><h1 class="title">BINGO!</h1>
    <div class="card"></div>
    </div>
    <div class="caller-page" v-show="caller">
      <button class="generate-number-button" @click="pickNumber">Next Number</button>
      <div class="current-number" v-if="showSpinner"><img class="loading-spinner" src="https://media.giphy.com/media/29JOa4o8Qc7W2S65sk/giphy.gif" alt="loading spinner"></div>
      <div class="current-number" v-else>{{ currentNumber }}</div>
      <h3>Numbers Drawn:</h3>
      <div class="drawn-numbers-wrapper">
      <div v-for="number in drawnNumbers" :key="number" class="drawn-number">{{ number }}</div>
      </div>
    </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Game',
  data() {
    return {
      startScreen: true,
      player: false,
      caller: false,
      cardSize: 25,
      card: null,
      numbers: null,
      cardContent: null,
      callerNums: [],
      currentNumber: '??',
      drawnNumbers: [],
      showSpinner: false,
    };
  },
  methods: {
    showCallerScreen() {
      this.startScreen = false;
      this.caller = true;
      //  setting up the array of numbers for the caller to pick from
      for (let i = 1; i <= 75; i += 1) {
        this.callerNums.push(i);
      }
    },
    pickNumber() {
      this.showSpinner = true;
      setTimeout(() => {
        const randomNumber = this.callerNums[(Math.floor(Math.random() * this.callerNums.length))];
        this.drawnNumbers.push(randomNumber);
        this.currentNumber = randomNumber;
        // ensuring that the same number can't be picked more than once
        this.callerNums = this.callerNums.filter((number) => !this.drawnNumbers.includes(number));
        this.showSpinner = false;
      }, 1000);
    },
    createCard() {
      this.startScreen = false;
      this.player = true;
      this.card = document.querySelector('.card');
      for (let i = 0; i < this.cardSize; i += 1) {
        this.card.appendChild(document.createElement('div')).classList.add('number');
      }
      this.numbers = document.querySelectorAll('.number');
      this.generateNumbers();
      this.numbers.forEach((number, i) => {
        const showNumber = number;
        // assign innerHTML of each square as number held at corresponding index in cardContent arr
        showNumber.innerHTML = this.cardContent[i];
        showNumber.addEventListener('click', this.handleClick);
        return showNumber;
      });
    },
    generateNumbers() {
      const cardContent = [];
      // create array of numbers 1 - 75
      let numberArray = [];
      for (let i = 1; i <= 75; i += 1) {
        numberArray.push(i);
      }
      while (cardContent.length !== this.cardSize) {
        const randomNumber = numberArray[(Math.floor(Math.random() * numberArray.length))];
        // push random numbers into cardNumbers array
        cardContent.push(randomNumber);
        // filter out any numbers that are already included in the cardContent array
        numberArray = numberArray.filter((number) => !cardContent.includes(number));
      }
      this.cardContent = cardContent;
    },
    handleClick(e) {
      if (e.target.style.backgroundColor === '') {
        e.target.style.backgroundColor = 'black';
        e.target.style.color = 'white';
      } else {
        e.target.style.backgroundColor = '';
        e.target.style.color = 'black';
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.title {
  color: black;
  font-size: 55px;
  margin: 10px 0px 0px 0px;
  letter-spacing: 5px;
  font-family: 'Indie Flower', cursive;
}
.game {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.game-in-play {
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 95vh;
  /* padding: 30px; */
}
.card-wrapper {
  margin: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background-color: red;
  border: solid black 1px;
  width: 40vw;
  height: 95vh;
}
.card {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  width: 40vw;
  height: 80vh;
}
.card >>> .number {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 18%;
  height: 18%;
  border: solid black 1px;
  margin: 0.5%;
  background-color: white;
  font-family: 'Arvo', serif;
  font-size: 30px;
  color: black;
  cursor: pointer;
}
.caller-page {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 90vw;
  height: 100vh;
  padding: 20px;
}
.current-number {
  display: flex;
  justify-content: center;
  align-items: center;
  padding-top: 5%;
  height: 50vh;
  font-size: 150px;
  margin-bottom: 20px;
}
.drawn-numbers-wrapper {
  display: flex;
  flex-wrap: wrap;
  /* justify-content: flex-end; */
  width: 100%;
  /* height: 60vh; */
}
.generate-number-button {
  height: 30px;
  width: 150px;
  font-size: 20px;
  background-color: chartreuse;
}
.drawn-number {
  display: flex;
  /* flex-direction: column; */
  justify-content: center;
  align-items: center;
  width: 50px;
  height: 50px;
  border: solid black 1px;
  margin: 0.5%;
  background-color: white;
  font-family: 'Arvo', serif;
  font-size: 30px;
  color: black;
  cursor: pointer;
}
</style>
