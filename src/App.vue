<template>
  <div id="app">
    <EOQ />
    <EOQDeficit />
  </div>
</template>

<script>
import EOQ from './components/EOQ.vue'
/*import EOQ from './components/EOQDeficit.vue'*/

export default {
  name: 'app',
  components: {
    EOQ
  }
}
</script>

<style>
body{
  background-color: #121525;
  margin: 0;
  top:0;
}
#app {
  width: 100%;
  height: 100vh;
  font-family: 'Avenir', Helvetica, Arial, sans-serif !important;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
}
@media only screen and (max-width: 999px) {
  #app {
    height: auto;
  }
}

</style>
