<template>
  <div id="app">
    <canvas width="300" height="150" style="border:1px solid #BBB;" v-insert-message="exampleContent"></canvas>
    <input type="text" v-model="exampleContent" />

    <!--
    <router-view></router-view>
    -->
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      activeIndex: '1',
      activeIndex2: '1',
      value1: true,
      value2: true,
      currentDate: new Date(),
      exampleContent: 'This is TEXT'
    }
  },
  methods: {
  },
  directives: {
    insertMessage: function (canvasElement, binding) {
      var ctx = canvasElement.getContext('2d')
      ctx.clearRect(0, 0, 300, 150)
      ctx.fillStyle = 'black'
      ctx.font = '20px Georgia'
      ctx.fillText(binding.value, 10, 50)
    }
  }
}

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
