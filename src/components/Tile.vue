<template>
  <div class="tile"> 
    <p class="number" @click="toggleTile(tile)"> {{ tile.number }} </p> 
  </div>
</template>

<script>

export default {
  props: ['tile'],
  emits: ['toggleTile'],
  data() {
    return {
      tilesSelected: 0
    }
  },
  methods: {
    toggleTile(tile) {
      console.log("Clicked")
      this.$emit('toggleTile', tile.id)
    }
  }
}

</script>

<style>
.tile {
  width: 100px;
  height: 100px;
  border: 2px solid gray;
}

.tile:hover {
  background-color: lightblue;
  cursor: pointer;
}

.number {
  font-size: 1.5em;
  font-weight: bold;
}
</style>