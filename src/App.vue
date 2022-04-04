<template>
  <div>
    <TabManager/>
  </div>
</template>

<script>
import TabManager from './components/TabManager.vue'

export default {
  name: 'App',
  components: {
    TabManager
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
