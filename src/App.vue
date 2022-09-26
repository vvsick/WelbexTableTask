<template>
  <div id="app" class="has-background-link-light">
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
  @import "https://cdn.jsdelivr.net/npm/bulma@0.9.4/css/bulma.min.css";
#app {
  font-family: "Lucida Sans Unicode";
  color: #e6e2da;
  font-weight: 600;
  font-size: 16px;
  width: 100%;
  height: 100vh;
}

body {
  margin: 0;
}
</style>