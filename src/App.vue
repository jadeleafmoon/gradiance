<template>
  <h1>Gradiance</h1>

  <p v-show="debug"> Correct tiles: {{ numberOfCorrectTiles }}</p>

  
  
  <div class="grid">
    <Tile 
    v-for="tile in tiles" 
    :tile="tile" 
    :key="tile.id"
    @toggle-tile="toggleTile(tile.id)"
    />
  </div>

  <h2 v-show="gameWon">You win! 😸</h2>

  <div class="debug" v-show="debug">
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
      debug: false,
      selectedTiles: [],
      tiles: [ 
        { number: 0, position: 0, isSelected: false, color: "black", id: 0},
        { number: 1, position: 5, isSelected: false, color: "#b3deff", id: 1}, 
        { number: 2, position: 1, isSelected: false, color: "#9ec7f7", id: 2}, 
        { number: 3, position: 4, isSelected: false, color: "#88b1ee", id: 3}, 
        { number: 4, position: 2, isSelected: false, color: "#739ae6", id: 4}, 
        { number: 5, position: 3, isSelected: false, color: "#5d83dd", id: 5}, 
      ],
    }
  },
  methods: {
    switchTiles() {
      let tile1 = this.selectedTiles[0]
      let tile2 = this.selectedTiles[1]
      let position1 = tile1.position
      let position2 = tile2.position
      
      tile1.position = position2
      tile2.position = position1

      tile1.isSelected = false
      tile2.isSelected = false

      let temp = this.tiles[position1]
      this.tiles[position1] = this.tiles[position2]
      this.tiles[position2] =  temp

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
    },
    generateTiles() {
      const result = [{
        number: 0,
        position: 0,
        isSelected: false,
        color: 'black',
        id: 0
      }]

      const numTiles = 25
      const colors = ["#b3deff", "#9ec7f7", "#88b1ee", "#739ae6", "#5d83dd"]
      let colorIdx = 0;

      for (let i = 1; i <= numTiles; i++) {
        if (colorIdx > 4) colorIdx = 0
        const color = colors[colorIdx]
        colorIdx += 1
        
        const newTile = {
          number: i,
          position: i,
          isSelected: false,
          color: color,
          id: i
        }

        result.push(newTile)
      }

      return result

    }
  },
  computed: {
    numberOfCorrectTiles() {
      let counter = 0;
      for (let i = 1; i < this.tiles.length; i++) {
        const tile = this.tiles[i];
        if (tile.number === i) counter++;
      }

      return counter
    },
    gameWon() {
      return this.numberOfCorrectTiles === this.tiles.length - 1
    }
  },
  beforeMount() {
    this.tiles = this.generateTiles()
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
  background: #efefef;
}

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  grid-gap: 0;
}

/* Firefox */
@media (prefers-color-scheme: dark) {
    :root {
        color-scheme: light;
    }
}

/* Chrome */
@media (forced-colors: active) {
    :root {
        color-scheme: light;
    }
}
</style>
