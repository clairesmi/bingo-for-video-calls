<template>
  <div id="app">
    <Game msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Game from './components/Game.vue';

export default {
  name: 'App',
  components: {
    Game,
  },
};
</script>

<style>
html {
  height: 100vh;
}
#app {
  font-family: 'Arvo', serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  height: 100vh;
}
</style>
