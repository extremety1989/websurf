<script setup>
import { ref, computed } from 'vue'
import WebSurf from './pages/WebSurf.vue'
import TopNavbar from './components/TopNavbar.vue'
const routes = {
  '/': WebSurf
}

const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] || NotFound
})
</script>

<template>
  <TopNavbar />
  <component :is="currentView" />
</template>