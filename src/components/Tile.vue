<template>
  <div 
    v-show="tile.id !== 0"
    @click="toggleTile(tile)"
    :class="{ 'tile': true, 'selected': tile.isSelected }"
    :style="{ backgroundColor: tile.color }"
    draggable="true"
  > 
    <p class="number"> {{ tile.number }}</p> 
  </div>
</template>

<script>

export default {
  props: ['tile', 'grid'],
  emits: ['toggleTile'],
  data() {
    return {
      tilesSelected: 0
    }
  },
  methods: {
    toggleTile(tile) {
      this.$emit('toggleTile', tile.id)
    }
  }
}

</script>

<style>
.tile {
  width: 100px;
  height: 100px;
}

.tile:hover {
  transform: scale(1.2);
  cursor: pointer;
}

.tile.selected {
  transform: scale(1.2);
  background-color: lightblue;
  border-color: lightblue;
}

.number {
  font-size: 1.5em;
  font-weight: bold;
}
</style>