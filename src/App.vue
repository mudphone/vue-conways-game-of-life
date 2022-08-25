<script setup>
  import * as _ from 'lodash'
  window._ = _
  import { ref } from 'vue'

  import GameCell from './components/GameCell.vue'
  import {getInitState, updateState} from './game/state'



  const DEFAULT_ROWS = 20
  const DEFAULT_COLS = 20
  const MAX_ROWS = 80
  const MAX_COLS = 80
  const INTERVAL_MILLIS = 500

  const DEFAULT_CELLS_SIZE = 20
  const MAX_CELLS_SIZE = 80


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
  const cellSize = ref(DEFAULT_CELLS_SIZE)

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
  <div class="game">
    <div class="controls">
      <button
        @click="restartGame"
        >Restart</button>
      Rows: {{ numRows }}
      <input
        type="range"
        min="2"
        :max="MAX_ROWS"
        v-model="numRows">
      Cols: {{ numCols }}
      <input
        min="2"
        :max="MAX_COLS"
        type="range"
        v-model="numCols">
      Cell Size: {{ cellSize }}
      <input
        min="10"
        :max="MAX_CELLS_SIZE"
        type="range"
        v-model="cellSize">
    </div>
    <div class="grid">
      <div
        v-for="rowIdx in _.range(_.size(gameState))"
        class="row"
        :key="rowIdx"
        >
        <game-cell
          v-for="colIdx in _.range(_.size(_.first(gameState)))"
          :key="colIdx"
          :cell-state="gameState[rowIdx][colIdx]"
          :style="'width: ' + cellSize + 'px; height: ' + cellSize + 'px;'"
          ></game-cell>
      </div>
    </div>
  </div>
</template>

<style scoped>

  .game {
    width: 100%;
  }

  .controls {
    position: fixed;
    top: 10px;
  }
  .grid {
    width: calc(100vw - 100px);
  }

  .row {
    display: flex;
    flex-direction: row;
  }
</style>