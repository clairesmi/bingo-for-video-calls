<template>
  <div class="game">
    <div class="start-screen" v-show="startScreen">
      <h1 class="title">BINGO!</h1>
      <h2>Are you...</h2>
      <div class="start-screen-buttons">
      <button class="button" @click="showCallerScreen">The bingo caller</button>
      <button class="button" @click="createCard">A bingo player</button>
      </div>
    </div>
    <div class="game-in-play" v-show="!startScreen">
    <div v-show="player" class="card-wrapper"><h1 class="card-title">BINGO!</h1>
    <div class="card"></div>
    </div>
    <div class="caller-page" v-show="caller">
      <button class="button" @click="pickNumber">Next Number</button>
      <div class="current-number" v-if="showSpinner"><img class="loading-spinner" src="https://media.giphy.com/media/29JOa4o8Qc7W2S65sk/giphy.gif" alt="loading spinner"></div>
      <div class="current-number" v-else>{{ currentNumber }}</div>
      <h3 class="numbers-drawn-header">Numbers Drawn:</h3>
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
      const vm = this;
      setTimeout(() => {
        const randomNumber = vm.callerNums[(Math.floor(Math.random() * this.callerNums.length))];
        vm.drawnNumbers.push(randomNumber);
        vm.currentNumber = randomNumber;
        // ensuring that the same number can't be picked more than once
        vm.callerNums = vm.callerNums.filter((number) => !vm.drawnNumbers.includes(number));
        vm.showSpinner = false;
      }, 800);
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
      // allow players to mark off their bingo card and reverse it if they make a mistake
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
.start-screen {
  display: flex;
  flex-direction: column;
  justify-content: center;
  justify-items: center;
}
.start-screen-buttons {
  display: flex;
  padding-top: 20px;
  width: 400px;
}
.title {
  color: black;
  font-size: 120px;
  margin: 10px 0px 0px 0px;
  letter-spacing: 5px;
  font-family: 'Indie Flower', cursive;
  padding-bottom: 20px;
}
.card-title {
  color: black;
  font-size: 55px;
  margin: 10px 0px 0px 0px;
  letter-spacing: 5px;
  font-family: 'Indie Flower', cursive;
  /* padding-bottom: 20px; */
}
.game {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
.game-in-play {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
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
  color: black;
}
.current-number {
  display: flex;
  justify-content: center;
  align-items: center;
  padding-top: 5%;
  height: 50vh;
  font-size: 150px;
  margin-bottom: 20px;
  font-family: 'Arvo', serif;
}
.drawn-numbers-wrapper {
  display: flex;
  flex-wrap: wrap;
  /* justify-content: flex-end; */
  width: 100%;
  height: 10%;
  /* overflow: scroll; */
}
.numbers-drawn-header {
  font-family: 'Indie Flower', cursive;
  font-size: 30px;
}
.button {
  height: 30px;
  width: 150px;
  margin: 10px;
  font-size: 20px;
  font-weight: bold;
  background-color: whitesmoke;
  font-family: 'Indie Flower', cursive;
  cursor: pointer;
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
