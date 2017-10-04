<template>
  <div id="app">
    <canvas id="world" style="border:1px solid #BBB;" v-insert-message="exampleContent"></canvas>
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
      ctx: '', // 適当に文字列で初期化
      exampleContent: 'This is TEXT'
    }
  },
  mounted () {
    var canvas = document.getElementById('world')
    canvas.width = document.body.clientWidth
    canvas.height = 500
  },
  created () {
    setInterval(() => {
      // console.log(((Math.random() * 100).toFixed(2)))
      this.run()
    }, 100)
  },
  methods: {
    run () {
      console.log('run')
    }
  },
  directives: {
    insertMessage: function (canvasElement, binding) {
      var ctx = canvasElement.getContext('2d')
      ctx.clearRect(0, 0, document.body.clientWidth, 400)
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
