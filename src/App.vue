<template>
  <h1>Hi!</h1>
  
  <input v-model="tile1" type="number"/>
  <input v-model="tile2" type="number"/>
  <button @click="switchTiles">Switch</button>
  <div class="grid">
    <Tile 
    v-for="tile in tiles" 
    :tile="tile" 
    :key="tile.id"
    @toggle-tile="toggleTile(tile.id)"
    />
  </div>
  <div class="debug" v-show="true">
    <div 
      class="debug-item" 
      v-for="tile in tiles"
      v-show="tile.id !== 0"
    > {{ tile.id }} , position: {{ tile.position }}
    </div>
  </div>

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
      selectedTiles: [],
      tiles: [ 
        { number: 0, position: 0, isSelected: false, id: 0},
        { number: 2, position: 1, isSelected: false, id: 2}, 
        { number: 4, position: 2, isSelected: false, id: 4}, 
        { number: 5, position: 3, isSelected: false, id: 5}, 
        { number: 3, position: 4, isSelected: false, id: 3}, 
        { number: 1, position: 5, isSelected: false, id: 1}, 
      ],
    }
  },
  methods: {
    switchTiles() {

      let tile1 = this.selectedTiles[0]
      let tile2 = this.selectedTiles[1]
      let tempPosition1 = tile1.position
      let tempPosition2 = tile2.position
      
      tile1.position = tempPosition2
      tile2.position = tempPosition1

      tile1.isSelected = false
      tile2.isSelected = false

      let temp = this.tiles[tempPosition1]
      this.tiles[tempPosition1] = this.tiles[tempPosition2]
      this.tiles[tempPosition2] =  temp

      this.selectedTiles = []
    },
    toggleTile(targetId) {
      for (const currentTile of this.tiles) {
        if (targetId === currentTile.id) {
          if(currentTile.isSelected) {
            currentTile.isSelected = false
            this.selectedTiles = this.selectedTiles.filter(item => item.id !== currentTile.id)
            break
          } else {
            currentTile.isSelected = true
            this.selectedTiles.push(currentTile)
            break
          }
        }
      }

      if (this.selectedTiles.length === 2) {
          this.switchTiles()
      }
    }
  },
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

.grid {
  display: flex;
}
</style>
