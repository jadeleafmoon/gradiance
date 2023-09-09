<template>
  <div class="tile"> <p class="number"> {{  number }}</p> </div>
</template>

<script>

export default {
  props: ['number']
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