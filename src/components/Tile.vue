<template>
  <div 
    @mousedown="toggleTile(tile)"
    :class="{ 'tile': true, 'selected': tile.isSelected, 'locked': tile.isLocked }"
    :style="{ backgroundColor: tile.color }"
    draggable="true"

  > 
    <p class="number" v-if="tile.isLocked">.</p>
    <p class="number" v-else> {{ " " }} </p> 
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
  /* width: 15vw;
  height: 15vw; */
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

.tile.locked {
  pointer-events: none;
}

.number {
  font-size: 1.1em;
  font-weight: bold;
  color: white;
}
</style>