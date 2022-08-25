<script setup>
  const props = defineProps({
    cellState: Boolean
  })
</script>

<template>
  <td
    class="cell"
    :class="{ 'alive': cellState }"
    >
    {{ cellState ? '' : '.' }}
  </td>
</template>

<style scoped>
  .alive {
    background-color: white;
  }
  .cell {
    width: 24px;
    height: 20px;
  }
</style>