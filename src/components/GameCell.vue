<script setup>
  const props = defineProps({
    cellState: Number
  })
</script>

<template>
  <td
    class="cell"
    :class="{ 'alive': cellState === 1 }"
    >
    {{ cellState }}
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