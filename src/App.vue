<template>
  <h1>Hi!</h1>
  <input />
  <input />
  <button @click="switchTiles">Switch</button>
  <Tile v-for="number in numbers" :number="number" />
</template>

<script>
import Tile from "./components/Tile.vue"


export default {
  name: 'App',
  components: { Tile },
  tile1: 0,
  tile2: 0,
  data() {
    return {
      numbers: [1, 2, 3]
    }
  },
  methods: {
    switchTiles() {
    }
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  background: #ddd;
}
</style>
