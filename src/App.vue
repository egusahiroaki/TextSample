<template>
  <div id="app">
    <canvas id="world" style="border:1px solid #BBB;"></canvas>
    <input type="text" v-model="initText" @input="updateText($event)" />

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
      canvas: '',
      ctx: '', // 適当に文字列で初期化
      initText: 'This is TEXT',
      positionX: 10,
      positionY: 50,
      speedX: 5,
      speedY: 5,
      text: ''
    }
  },
  mounted () {
    this.canvas = document.getElementById('world')
    this.canvas.width = document.body.clientWidth
    this.canvas.height = 500
    this.ctx = this.canvas.getContext('2d')
    this.ctx.fillStyle = 'black'
    this.ctx.font = '20px Georgia'
  },
  created () {
    // 初期化
    setInterval(() => {
      this.ctx.clearRect(0, 0, document.body.clientWidth, 500)
      // console.log(((Math.random() * 100).toFixed(2)))
      this.run()
    }, 33)
  },
  methods: {
    run () {
      if (this.positionX > this.canvas.width || this.positionX < 0) {
        this.speedX *= -1
      }

      if (this.positionY > 500 || this.positionY < 0) {
        this.speedY *= -1
      }

      this.positionX = this.positionX + this.speedX
      this.positionY = this.positionY + this.speedY
      this.ctx.fillText(this.text, this.positionX, this.positionY)
    },
    updateText (e) {
      this.text = e.target.value
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
