<template>
  <div 
    v-show="tile.id !== 0"
    @mousedown="toggleTile(tile)"
    :class="{ 'tile': true, 'selected': tile.isSelected }"
    :style="{ backgroundColor: tile.color }"
    draggable="true"
  > 
    <p class="number" v-show="debugOn"> {{ tile.number }}</p> 
  </div>
</template>

<script>

export default {
  props: ['tile', 'grid'],
  emits: ['toggleTile'],
  data() {
    return {
      debugOn: true,
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
  transition: all .1s ease-in-out;
  box-shadow: rgba(0, 0, 0, 0.05) 0px 1px 2px 0px;

  display: flex;
  justify-content: center;
  align-items: center;
}

.tile:hover {
  transform: scale(1.1);
  cursor: pointer;
}

.tile.selected {
  transform: scale(1.2);
  background-color: lightblue;
  border-color: lightblue;
}

.number {
  font-size: 1.1em;
  font-weight: bold;
  color: white;
}
</style>