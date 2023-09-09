<template>
  <h1>Hi!</h1>
  <input v-model="tile1" type="number"/>
  <input v-model="tile2" type="number"/>
  <button @click="switchTiles">Switch</button>
  <Tile 
    v-for="tile in tiles" 
    :tile="tile" 
    :key="tile.id"
    @toggle-tile="toggleTile(tile.id)"
  />
</template>

<script>
import Tile from "./components/Tile.vue"


export default {
  name: 'App',
  components: { Tile },
  data() {
    return {
      tile1: 0,
      tile2: 0,
      tiles: [ 
        { number: 1, position: 1, isSelected: false, id: 1}, 
        { number: 2, position: 2, isSelected: false, id: 2}, 
        { number: 3, position: 3, isSelected: false, id: 3}, 
      ],
      grid: [

      ],
    }
  },
  methods: {
    switchTiles() {
      const foundTile1 = this.tiles.find(item => item.number === this.tile1)
      const foundTile2 = this.tiles.find(item => item.number === this.tile2)
      let temp = foundTile1.position
      
      foundTile1.position = foundTile2.position
      foundTile2.position = temp

      this.tile1 = 0
      this.tile2 = 0
    },
    toggleTile(targetId) {
      for (const currentTile of this.tiles) {
        if (targetId === currentTile.id) {
          currentTile.isSelected = !currentTile.isSelected
          break
        }
      }
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
