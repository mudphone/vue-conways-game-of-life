<script setup>
  import * as _ from 'lodash'
  window._ = _
  import { ref } from 'vue'

  import GameCell from './components/GameCell.vue'
  import {getInitState, updateState} from './game/state'



  const ROWS = 20
  const COLS = 20
  const INTERVAL_MILLIS = 2000


  const iterateGame = (rows, cols, state) => {
    state.value = updateState(rows, cols, state.value)
  }


  const startGame = (state, intervalMillis) => {
    const rows = state.value.length
    const cols = state.value[0].length
    setInterval(() => iterateGame(rows, cols, state), intervalMillis)
  }


  const initialState = getInitState(ROWS, COLS)
  const gameState = ref(initialState)
  startGame(gameState, INTERVAL_MILLIS)
</script>

<template>
  <table>
    <tr 
      v-for="rowIdx in _.range(ROWS)"
      :key="rowIdx"
      >
      <game-cell
        v-for="colIdx in _.range(COLS)"
        :key="colIdx"
        :cell-state="gameState[rowIdx][colIdx]"
        ></game-cell>
    </tr>
  </table>
</template>