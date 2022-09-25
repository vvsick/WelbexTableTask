<template>
  <div id="app">
    <TableModule />
  </div>
</template>

<script>
import TableModule from './components/TableModule.vue'

export default {
  name: 'App',
  components: {
    TableModule
  }
}
</script>

<style>
#app {
  font-family: "Lucida Sans Unicode";
  color: #e6e2da;
  font-weight: 600;
  font-size: 16px;
  width: 100%;
  height: 100vh;
  background: #8392b4;
}

body {
  margin: 0;
}
</style>