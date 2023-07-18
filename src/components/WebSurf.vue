<script setup>
import { ref } from 'vue'

// copied from https://www.w3schools.com/tags/canvas_arc.asp
const canvas = document.getElementsByClassName("canvas");
const ctx = canvas.getContext("2d");
///////////////////////////////////////////////////////////////////
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
const nodes = ref([])


function add(x, y) {
    if(!matrix.value[0]){
        matrix.value[0] = []
    }
    matrix.value[0].push(0)
    nodes.value.push({x: x, y: y}) 
    ctx.beginPath();
    ctx.arc(x, y, 50, 0, 2 * Math.PI);
    ctx.fillStyle = "orange";
    ctx.fill(); 
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
      add(i+1, y+1)
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
<canvas></canvas>
</template>

<style scoped>

</style>
