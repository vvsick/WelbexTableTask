<template>
  <div id="app">
    <TableModule />
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
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
  color: #2c3e50;
  font-size: 16px;
  width: 100%;
  height: 100vh;
  background: #e6e3ac;
}

body {
  margin: 0;
}
</style>