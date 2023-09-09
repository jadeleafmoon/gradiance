<template>
  <div class="tile"> <p class="number" @click="toggleTile"> {{ tile.number }} </p> </div>
</template>

<script>

export default {
  props: ['tile'],
  data() {
    return {
      tilesSelected: 0
    }
  },
  methods: {
    toggleTile() {

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

.number {
  font-size: 1.5em;
  font-weight: bold;
}
</style>