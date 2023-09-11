<template>
  <div class="container">
    <h1 class="game-title">Gradiance</h1>

    <div class="grid-wrapper">
      <div class="grid">
        <Tile 
          v-for="tile in tiles" 
          :tile="tile" 
          :key="tile.id"
          class="tile"
          @toggle-tile="toggleTile(tile.id)"
        />
      </div>

    </div>

    <h2 v-show="gameWon">You win! 😸</h2>

    <button @click="playAgain" v-show="gameWon">Play again</button>

    <br>

    <button @click="solveGame">Solve</button>
    <button @click="toggleDebug" >Debug</button>




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

  </div>


</template>

<script>
import Tile from "./components/Tile.vue"
import ColorPalettes from "./assets/ColorPalettes.js"
import LockedTilesPatterns from "./assets/LockedTilesPatterns"


export default {
  name: 'App',
  components: { Tile },
  data() {
    return {
      debugOn: false,
      firstGame: true,
      selectedTiles: [],
      tiles: [],
      colorPaletteIdx: 0,
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

      const colors = ColorPalettes[this.colorPaletteIdx]

      if (this.colorPaletteIdx === ColorPalettes.length - 1) {
        this.colorPaletteIdx = 0
      } else {
        this.colorPaletteIdx += 1
      }
 

      let randomIndex = Math.floor(Math.random() * (LockedTilesPatterns.length - 1))

      if (this.firstGame) {
        randomIndex = 0
        this.firstGame = false
      }

      const lockedTiles = LockedTilesPatterns[randomIndex]

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

      return copy
    },
    
    solveGame() {
      let result = []

      for (let i = 0; i < this.tiles.length; i++) {
        let correctPosition = this.tiles[i].number
        this.tiles[i].position = correctPosition
        result[correctPosition] = this.tiles[i]
      }

      this.tiles = result

      return result
    },
    toggleDebug() {
      this.debugOn = !this.debugOn
    },
    playAgain() {
      this.selectedTiles = []
      this.tiles = this.generateTiles()
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
  margin-top: 20px;
}

body {
  background: #efefef;
  margin: 0 0;
}

.container {
  margin: 0px auto;
}

.grid-wrapper {
  max-width: 400px;
  margin: 10px auto;
  padding: 5px 25px;
  text-align: center;
}


.grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-gap: 0;
  aspect-ratio: 1 / 1;

}

.tile {
  /* aspect-ratio: 1 / 1; */

}

button {
  padding: 5px 5px;
  margin: 0 5px 10px 5px;
  cursor: pointer;

}

.game-title {
  text-transform: uppercase;
  /* color: #666; */
  letter-spacing: 0.15em;
  background: -webkit-linear-gradient(#A784D8, #404877);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
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
