<script setup>
  import { ref, onMounted } from 'vue'
  import { Network } from 'vis-network';
  import { DataSet } from 'vis-data'
  const table = ref("")
  const container = ref(null);
  const matrix = ref([])
  const network = ref(null)
  const edges = ref([])
  const nodes = ref([])
  
  const nodes_data = ref([])
  const edges_data = ref([])

  const current_selected = ref(-1)

  onMounted(() => {


    // mark(0, 1)
    // mark(0, 4)
    // mark(1, 2)
    // mark(1, 3)
    // mark(1, 4)
    // mark(2, 3)
    // mark(3, 4)
   

    const canvas = document.getElementsByTagName("canvas")
 

    window.addEventListener("keydown", (event) => {
          if(event.key === "a" || event.key === "A"){
            add()
            displayGraph()
          }
        })
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
  //////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


  function add() {
    if (!matrix.value[0]) {
      matrix.value[0] = []
    }
    matrix.value[0].push(0)
    const j = matrix.value[0].length
    nodes_data.value = []
    for (let i = 0; i < j; i++) {
         nodes_data.value.push({id: i, label: `${i}`})
    }
    //////////////////////////////////////////////////////////////////

    for (let i = 1; i < j; i++) {
      if (!matrix.value[i]) {
        matrix.value[i] = []
      }
      while (matrix.value[i].length < j) {
        matrix.value[i].push(0)
      }
    }
  }

  function remove(index) {
    if (matrix.value[0].length === 1) return
    for (let i = 0; i < matrix.value[0].length; i++) {
      matrix.value[i].splice(index, 1);
    }
    matrix.value = matrix.value.slice(0)
    matrix.value.splice(index, 1)
  }

  function mark(i, j) {
    if (matrix.value[i][j] === 1) {
      alert(`Duplicate connection (${i},${j}) of (${j},${i})`)
    } else {
      edges_data.value.push({from: i, to: j})
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
   
    nodes.value = new DataSet(nodes_data.value)
    edges.value = new DataSet(edges_data.value)
    const datas = {
      nodes: nodes.value,
      edges: edges.value,
    };  


    const locales = {
      en: {
        edit: 'Edit',
        del: 'Delete selected',
        back: 'Back',
        addNode: 'Add Node',
        addEdge: 'Add Edge',
        editNode: 'Edit Node',
        editEdge: 'Edit Edge',
        addDescription: 'Click in an empty space to place a new node.',
        edgeDescription: 'Click on a node and drag the edge to another node to connect them.',
        editEdgeDescription: 'Click on the control points and drag them to a node to connect to it.',
        createEdgeError: 'Cannot link edges to a cluster.',
        deleteClusterError: 'Clusters cannot be deleted.',
        editClusterError: 'Clusters cannot be edited.'
      }
    }
        const options = {
          autoResize: true,
          height: '100%',
          width: '100%',
          clickToUse: true,
          locale: "en",
          locales: locales,
          configure: {
            enabled: false,
            filter: 'nodes,edges',
            container: container.value,
            showButton: true
          }
        };
        network.value = new Network(container.value, datas, options);
        network.value.on("doubleClick", (event)=>{
          if(event.edges.length > 0){
            edges.value.remove(event.edges[0])
          }else if(event.nodes.length > 0){
            nodes.value.remove(event.nodes[0])
            remove(event.nodes[0])
          }
        })
        network.value.on('selectEdge', function(event){
          console.log(event.edges[0]);
        });
        network.value.on('selectNode', function(event){
          current_selected.value = event.nodes[0]
          console.log(event.nodes[0]);
        });
      
  }

</script>

<template>
  <div ref="container" id="container"></div>
  <span v-if="current_selected !== -1" id="current_selected">{{ current_selected }}</span>
</template>

<style scoped>
 #container {
    z-index: 0;
 }
 #current_selected {
   position: absolute;
   padding: 50px;
   bottom: 0;
   z-index: 100;
 }
</style>