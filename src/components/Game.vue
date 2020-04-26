<template>
  <div class="game">
    <div v-show="startScreen">
      <h1 class="title">BINGO!</h1>
      <h2>Are you...</h2>
      <button @click="showCaller">The bingo caller</button>
      <button @click="createCard">A bingo player</button>
    </div>
    <div v-show="!startScreen">
    <div v-show="player" class="card-wrapper"><h1 class="title">BINGO!</h1>
    <div class="card"></div>
    </div>
    <div v-show="caller">
      <button @click="pickNumber">Next Number</button>
      <div>{{ currentNumber }}</div>
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
      currentNumber: null,
    };
  },
  methods: {
    showCaller() {
      this.startScreen = false;
      this.caller = true;
    },
    pickNumber() {
      console.log('picked');
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
</style>
