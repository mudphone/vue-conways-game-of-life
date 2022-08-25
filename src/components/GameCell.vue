<script setup>
  const emit = defineEmits(['toggleCell'])


  const props = defineProps({
    cellState: Boolean,
  })

  
  const cellClick = () => {
    emit('toggleCell')
  }
</script>

<template>
  <div
    class="cell"
    :class="{ 'alive': cellState, 'empty': !cellState }"
    @click="cellClick"
    >
  </div>
</template>

<style scoped>
  /* .cell {
    width: 20px;
    height: 20px;
    border: 0.1px solid white;
  } */
  .cell {
    border: 0.05px dotted white;
  }

  .alive {
    background-color: white;
  }

  .alive:hover {
    background-color: red;
    border: none;
  }

  .empty {
    background-color: darkgray;
    opacity: 0.1;
  }

  .empty:hover {
    background-color: green;
    opacity: 1.0;
    border: none;
  }
</style>