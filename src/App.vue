<template>
  <div id="app">
    <canvas id="world" style="border:1px solid #BBB;"></canvas>
    <input type="text" v-model="realtimeText" @input="bindText($event)"/>
    <input type="text" v-model="initText" @input="updateText($event)" @keyup.enter="submit"/>

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
      realtimeText: 'This is realtime TEXT',
      initText: 'This is TEXT',
      currentBindText: '',
      currentInputText: '',
      items: [] // canvas中にあるテキスト
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
      if (this.items.length !== 0) {
        this.run()
      }
      this.speedX += 0.1
      this.ctx.font = this.speedX + 'px Georgia'
      this.ctx.fillText(this.realtimeText, 50, 50)
    }, 33)
  },
  methods: {
    submit () { // enterを押すと入力している文字は消えるようにする
      var move = {
        width: (this.currentInputText.length - 1) * 8,
        height: 20,
        x: this.canvas.width / 2,
        y: this.canvas.height / 2,
        speedX: Math.random() * 10 - 5,
        speedY: Math.random() * 10 - 5,
        text: this.currentInputText
      }
      this.items.push(move)
      this.currentInputText = ''
      this.initText = ''
    },
    run () {
      this.items.forEach((item) => {
        if (item.x > this.canvas.width - item.width || item.x < 0) {
          item.speedX *= -1
        }

        if (item.y > 500 - item.height || item.y < item.height) {
          item.speedY *= -1
        }

        item.x = item.x + item.speedX
        item.y = item.y + item.speedY
        this.ctx.fillText(item.text, item.x, item.y)
      })
    },
    bindText (e) {
      this.currentBindText = e.target.value
      console.log(this.currentBindText)
    },
    updateText (e) {
      this.currentInputText = e.target.value
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
