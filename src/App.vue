<template>
  <h1>Gradiance</h1>

  <h2 v-show="gameWon">You win! 😸</h2>

  <div v-show="debugOn">
    <button @click="almostSolve">Solve</button>
    <p> Correct tiles: {{ numberOfCorrectTiles }}</p>
  </div>
  
  
  <div class="grid">
    <Tile 
      v-for="tile in tiles" 
      :tile="tile" 
      :key="tile.id"
      class="tile"
      @toggle-tile="toggleTile(tile.id)"
    />
  </div>



  <div class="debugOn" v-show="debugOn">
    <pre>{{ selectedTiles }}</pre>
    <p> array length: {{ tiles.length }} </p>
    <div 
      class="debugOn-item" 
      v-for="tile in tiles"
    > 
      {{ tile.id }} , position: {{ tile.position }}, 
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
      debugOn: true,
      selectedTiles: [],
      tiles: [],
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
      let result = []

      const colors = [
        "#fbef5e", "#F0D363", "#E7B872", "#DD9C84", "#D5819A",
        "#DAE769", "#D1C972", "#CAAB80", "#C38E90", "#BD71A2",
        "#BBDF7D", "#B4BF85", "#ADA090", "#A9819C", "#A762AB",
        "#9CD792", "#95B697", "#9294A1", "#9073AB", "#9052B5",
        "#80D0A9", "#79ACAE", "#7689B2", "#7566B7", "#7743BF"
      ]

      const lockedTiles = [
        1, 1, 1, 1, 1,
        1, 0, 0, 0, 1,
        1, 0, 0, 0, 1,
        1, 0, 0, 0, 1,
        1, 1, 1, 1, 1,
      ]

      let colorIdx = 0

      for (let i = 0; i < colors.length; i++) {

        const currentColor = colors[i]

        const isLocked = lockedTiles[i] === 1

        const newTile = {
          number: i,
          position: i,
          isSelected: false,
          color: currentColor,
          id: i,
          isLocked: isLocked,
        }

        result.push(newTile)
      }

      result = this.shuffleTiles(result, lockedTiles)
      
      return result
    },
    shuffleArray(arr) {
      let copy = arr.slice()

      for (let i = 0; i < copy.length; i++) {
        const randomIndex = Math.floor(Math.random() * arr.length)

        let temp = copy[i]
        copy[i] = copy[randomIndex]
        copy[randomIndex] = temp

      }

      return copy
    },
    shuffleTiles(arr, lockedTiles) {


      let result = []
      let copy = arr.slice()

      for (let i = 0; i < copy.length; i++) {
        let isLocked = lockedTiles[i] === 1

        
        if (!isLocked) {
          let swapped = false
          // swap with random unlocked tile
          while (!swapped) {
            const randomIndex = Math.floor(Math.random() * arr.length)
            if (lockedTiles[randomIndex] !== 1) {

              // swap tiles 
              let temp = copy[i]
              let tempPosition = copy[i].position

              copy[i].position = copy[randomIndex].position
              copy[randomIndex].position = tempPosition

              copy[i] = copy[randomIndex]
              copy[randomIndex] = temp
              swapped = true
            } 
          }
          

        }

      }

      console.log("copy", copy)
      return copy
    },
    
    almostSolve() {
      let result = []

      for (let i = 0; i < this.tiles.length; i++) {
        let correctPosition = this.tiles[i].number
        this.tiles[i].position = correctPosition
        result[correctPosition] = this.tiles[i]
      }

      this.tiles = result

      return result
    }
  },
  computed: {
    numberOfCorrectTiles() {
      let counter = 0;
      for (let i = 0; i < this.tiles.length; i++) {
        const tile = this.tiles[i];
        if (tile.number === i) counter++;
      }

      return counter
    },
    gameWon() {
      return this.numberOfCorrectTiles === this.tiles.length
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
  grid-template-columns: repeat(5, 1fr);
  grid-gap: 0;
}

.tile {
  /* width: 15vw;
  height: 15vw; */
}

/* Firefox */
@media (prefers-color-scheme: dark) {
    :root {
        color-scheme: light only;
    }
}

/* Chrome */
@media (forced-colors: active) {
    :root {
        color-scheme: light only;
    }
}
</style>
