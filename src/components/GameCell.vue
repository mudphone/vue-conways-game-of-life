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
    :class="{ 'alive': cellState }"
    @click="cellClick"
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