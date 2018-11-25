<template>
  <div id="app">
      <span class="title">TIC TAC TOE</span>
      <Board/>
  </div>
</template>

<script>
import Board from './components/Board'

export default {
  name: 'App',
  components: {
    Board
  }
}
</script>

<style>
body {
  background-color: #fff;
  color: #fff;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  margin: 0px;
}
#app {
  margin: 0 auto;
  max-width: 475px;
}
.title {
  font-family: 'Titan One', Helvetica, sans-serif;
  color: black;
  font-size: 3em;
}
</style>
