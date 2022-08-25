<script setup>
  import * as _ from 'lodash'
  window._ = _
  import { ref } from 'vue'

  import GameCell from './components/GameCell.vue'
  import {getInitState, updateState} from './game/state'



  const DEFAULT_ROWS = 20
  const DEFAULT_COLS = 20
  const INTERVAL_MILLIS = 500


  const iterateGame = (rows, cols, state) => {
    state.value = updateState(rows, cols, state.value)
  }


  const startGame = (state, intervalMillis) => {
    const rows = _.size(state.value)
    const cols = _.size(_.first(state.value))
    return setInterval(() => iterateGame(rows, cols, state), intervalMillis)
  }


  const numRows = ref(DEFAULT_ROWS)
  const numCols = ref(DEFAULT_COLS)

  const initialState = getInitState(numRows.value, numCols.value)
  const gameState = ref(initialState)
  let timer = startGame(gameState, INTERVAL_MILLIS)


  const restartGame = () => {
    clearTimeout(timer)
    const initialState = getInitState(numRows.value, numCols.value)
    gameState.value = initialState
    timer = startGame(gameState, INTERVAL_MILLIS)
  }
</script>

<template>
  <div>
    <button
      @click="restartGame"
      >Restart</button>
    Rows: {{ numRows }}
    <input
      type="range"
      min="2"
      max="100"
      v-model="numRows">
    Cols: {{ numCols }}
    <input
      min="2"
      max="100"
      type="range"
      v-model="numCols">
    <table>
      <tr 
        v-for="rowIdx in _.range(_.size(gameState))"
        :key="rowIdx"
        >
        <game-cell
          v-for="colIdx in _.range(_.size(_.first(gameState)))"
          :key="colIdx"
          :cell-state="gameState[rowIdx][colIdx]"
          ></game-cell>
      </tr>
    </table>
  </div>
</template>