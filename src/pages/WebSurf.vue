<script setup>

  import { ref, onMounted, watch } from 'vue'
  import BottomNavbar from '../components/BottomNavbar.vue'
  let canvas = document.getElementById("canvas")

  const ctx = ref(null);
  const matrix = ref([])
  const edges = ref([])
  const nodes = ref([])
  const isSelected = ref(-1)

  onMounted(() => {
    canvas.width = window.innerWidth
    canvas.height = window.innerHeight - 65
    ctx.value = canvas.getContext("2d")
    ctx.value.beginPath();
    ctx.value.rect(1600, 20, 150, 100);
    ctx.value.stroke();
    displayGraph()
  })


  // copied from 
  // https://stackoverflow.com/questions/66788019/vuejs-dragging-an-element-on-occasion-causes-it-to-stick-to-the-mouse-pointer
  function mousedown(e) {
    window.addEventListener('mousemove', mousemove)
    window.addEventListener('mouseup', mouseup)

    let prevX = e.clientX
    let prevY = e.clientY

    function mousemove(e) {
      const newX = prevX - e.clientX
      const newY = prevY - e.clientY
      const disc = event.target
      const rect = disc.getBoundingClientRect()
      disc.style.left = rect.left - newX + 'px'
      disc.style.top = rect.top - newY + 'px'
      prevX = e.clientX
      prevY = e.clientY
    }
    function mouseup() {
      window.removeEventListener('mousemove', mousemove)
      window.removeEventListener('mouseup', mouseup)
    }
  }
 
  function add() {

    if (!matrix.value[0]) {
      matrix.value[0] = []
    }
    matrix.value[0].push(0)
    for (let i = 1; i < matrix.value[0].length; i++) {
      if (!matrix.value[i]) {
        matrix.value[i] = []
      }
      while (matrix.value[i].length < j) {
        matrix.value[i].push(0)
      }
    }
  }

  function remove(index) {
    if (!matrix.value[0] || matrix.value[0].length === 0) {
      console.log("empty matrix");
      return
    }
    for (let i = 0; i < matrix.value[0].length + 1; i++) {
      matrix.value[i].splice(index, 1);
    }
    matrix.value = matrix.value.slice(0)
    matrix.value.splice(index, 1)
    isSelected.value = -1
  }

  function mark(i, j) {
    if (matrix.value[i][j] === 1) {
      console.log(`Duplicate connection (${i},${j}) of (${j},${i})`)
    } else {
      matrix.value[i][j] = 1
      matrix.value[j][i] = 1
    }
  }

  function unmark(i, j) {
    matrix.value[i][j] = 0
    matrix.value[j][i] = 0
  }

  function displayGraph() {

    // only for debugging, it is performance heavy
    console.table(JSON.parse(JSON.stringify(matrix.value)))

  }
function selectedActionGraph(id) {
  isSelected.value = id
}

</script>

<template>
    <canvas class="bg-slate-50" id="canvas" ref="canvas"></canvas>
    <BottomNavbar @drag="drag" @selected="selectedActionGraph" draggable="true" />
</template>