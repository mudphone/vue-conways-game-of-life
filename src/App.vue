<script setup>
  import { ref } from 'vue'
  import * as _ from 'lodash'

  const D = 0 // dead
  const A = 1 // alive

  const ROWS = 20
  const COLS = 20

  const getInitState = () => {
    const s = []
    _.forEach(_.range(ROWS), () => {
      s.push(_.fill(Array(COLS), D))
    })
    _.forEach(_.range(ROWS), r => {
      _.forEach(_.range(COLS), c => {
        if (_.random(10) >= 5) {
          s[r][c] = A
        }
      })
    })
    return s
  }

  const initialState = getInitState()
  const gameState = ref(initialState)

  const iterateGame = () => {
    updateState(_.cloneDeep(gameState.value))
  }

  const updateState = (state) => {
    _.forEach(_.range(ROWS), r => {
      _.forEach(_.range(COLS), c => {
        gameState.value[r][c] = cellState(r, c, state)
      })
    })
  }

  const cellState = (rowIdx, colIdx, state) => {
    const cell = state[rowIdx][colIdx]
    const neighbors = neighborsFor(rowIdx, colIdx)
    if (cell === A) {
      // cell is alive
      let numLiveNeighbors = 0
      _.forEach(neighbors, ([r, c]) => {
        if (state[r][c] === A) {
          numLiveNeighbors++
        }
        if (numLiveNeighbors > 3) {
          return false // break
        }
      })
      if (numLiveNeighbors === 2 || numLiveNeighbors === 3) {
        return A
      }

    } else {
      // cell is dead
      let numLiveNeighbors = 0
      _.forEach(neighbors, ([r, c]) => {
        if (state[r][c] === A) {
          numLiveNeighbors++
        }
        if (numLiveNeighbors > 4) {
          return false // break
        }
      })
      if (numLiveNeighbors === 3) {
        return A
      }

    }

    return D
  }

  const neighborsFor = (rowIdx, colIdx) => {
    const n = []
    for (let r = rowIdx - 1; r <= rowIdx + 1; r++) {
      for (let c = colIdx - 1; c <= colIdx + 1; c++) {
        if (r >= 0 && r < ROWS && c >= 0 && c <= COLS && !_.isEqual([r, c], [rowIdx, colIdx])) {
          n.push([r, c])
        }
      }
    }
    return n
  }

  setInterval(iterateGame, 2000)
</script>

<template>
  <table>
    <tr 
      v-for="rowIdx in _.range(ROWS)"
      :key="rowIdx"
      >
      <td
        v-for="colIdx in _.range(COLS)"
        :key="colIdx"
        >{{ gameState[rowIdx][colIdx] }}</td>
    </tr>
  </table>
</template>