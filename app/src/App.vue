<template>
  <div id="app">
    <h1>Chapel Hill Incidents</h1>
    <GoogleMap />
  </div>
</template>

<script>
import GoogleMap from '@/components/GoogleMap.vue'

export default {
  name: 'app',
  components: {
    GoogleMap
  },
  mounted() {
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
