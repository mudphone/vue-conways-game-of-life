<script setup>
  const props = defineProps({
    cellState: Boolean
  })
</script>

<template>
  <div
    class="cell"
    :class="{ 'alive': cellState }"
    >
    {{ cellState ? '' : '.' }}
  </div>
</template>

<style scoped>
  .alive {
    background-color: white;
  }
  /* .cell {
    width: 20px;
    height: 20px;
  } */
</style>