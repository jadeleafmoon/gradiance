<template>
  <div 
    @click="toggleTile(tile)"
    :class="{ 'tile': true, 'selected': tile.isSelected }"
  > 
    <p class="number"> {{ tile.number }} {{ tile.isSelected }} </p> 
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
  border: 4px solid lightblue;
  cursor: pointer;
}

.tile.selected {
  background-color: lightblue;
}

.number {
  font-size: 1.5em;
  font-weight: bold;
}
</style>