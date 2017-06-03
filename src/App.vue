<template>
  <div id="app">
  	<drag></drag>
  </div>
</template>

<script>
import drag from './vue-drag.vue'
export default {
  name: 'app',
  components: {
  	drag
  }
}
</script>

<style lang="scss">
* {
  padding: 0;
  margin: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding-top: 40px;
  padding-bottom: 40px;
}
</style>
