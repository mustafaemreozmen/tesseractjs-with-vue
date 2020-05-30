<template>
<div>
    <ocrModule :regions="this.rectangles"/>
</div>
</template>

<script>
import ocrModule from './components/ocrModule/ocrModule.vue'

export default {
  name: 'App',
  components: {
    ocrModule
  },
  data(){
    return {
      rectangles: [
        {
          left: 6,
          top: 20,
          width: 400,
          height: 50
        },
        {
          left: 170,
          top: 130,
          width: 400,
          height: 50
        }
      ]
    }
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
