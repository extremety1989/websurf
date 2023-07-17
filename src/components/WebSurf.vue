<script setup>
import { ref } from 'vue'

const global_j = ref(0)

const matrix = ref([[0]])

function add() {
  let i = matrix.value[0].length
  while (i <= global_j) {
    matrix.value[i].push(1)
  }

  global_j.value += 1
  matrix.value[global_j.value] = []
  i = global_j.value
  while (i <= global_j.value+1) {
      matrix.value[global_j.value].push(0)
      i +=1
  }

}

function mark(i, j) {
  matrix.value[i][j] = 1
  matrix.value[j][i] = 1
}

function unmark(i, j) {
  matrix.value[i][j] = 0
  matrix.value[j][i] = 0
}

function displayMatrix() {
  console.table(matrix.value)
}
displayMatrix()
add()
displayMatrix()
add()
displayMatrix()
</script>

<template>
<canvas></canvas>
</template>

<style scoped>
</style>
