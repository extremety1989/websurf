<script setup>
import { ref } from 'vue'

const table = ref("")

// copied from 
// https://stackoverflow.com/questions/66788019/vuejs-dragging-an-element-on-occasion-causes-it-to-stick-to-the-mouse-pointer
function mousedown (e) {
  window.addEventListener('mousemove', mousemove)
  window.addEventListener('mouseup', mouseup)

  let prevX = e.clientX
  let prevY = e.clientY

  function mousemove (e) {
    const newX = prevX - e.clientX
    const newY = prevY - e.clientY
    const disc = event.target
    const rect = disc.getBoundingClientRect()
    disc.style.left = rect.left - newX + 'px'
    disc.style.top = rect.top - newY + 'px'
    prevX = e.clientX
    prevY = e.clientY
  }
  function mouseup () {
    window.removeEventListener('mousemove', mousemove)
    window.removeEventListener('mouseup', mouseup)
  }
}
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

const matrix = ref([])
const vertex = ref([])
vertex.value.push({x: window.innerWidth / 2, y: window.innerHeight / 2}) 

function add() {
    if(!matrix.value[0]){
        matrix.value[0] = []
    }
    matrix.value[0].push(0)
    const j = matrix.value[0].length
    for(let i = 1; i < j; i++){
      if(!matrix.value[i]){
        matrix.value[i] = []
      }
      while (matrix.value[i].length < j) {
          matrix.value[i].push(0)
      }
    }
}

function remove(index) {
   if(matrix.value[0].length === 1) return
   for(let i = 0; i < matrix.value[0].length; i++){
      matrix.value[i].splice(index, 1);
   }
   matrix.value = matrix.value.slice(0)
   matrix.value.splice(index, 1)
}

function mark(i, j) {
  if(matrix.value[i][j] === 1){
      alert(`Duplicate connection (${i},${j}) of (${j},${i})`)
  }else{
    matrix.value[i][j] = 1
    matrix.value[j][i] = 1
  }
}

function unmark(i, j) {
  matrix.value[i][j] = 0
  matrix.value[j][i] = 0
}

function displayMatrix() {
  console.table(matrix.value)
}

const number_of_nodes = 5
for (let i = 0; i < number_of_nodes; i++) {
      add()
}

mark(0, 1)
mark(0, 4)
mark(1, 2)
mark(1, 3)
mark(1, 4)
mark(2, 3)
mark(3, 4)

displayMatrix()
</script>

<template>

    <table v-for="line in matrix">
      <td style="padding-left: 50px;" v-for="cell in line">
        {{cell}}
      </td>
    </table>

</template>

<style scoped>

</style>
