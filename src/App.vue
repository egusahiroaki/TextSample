<template>
  <div id="app">
    <canvas id="world" style="border:1px solid #BBB;"></canvas>
    <input type="text" v-model="realtimeText" @input="bindText($event)" @keyup.enter="bindSubmit"/>

<!--
     <input type="text" v-model="initText" @input="updateText($event)" @keyup.enter="submit"/>
 -->
    <input type="text" v-model="initText" @keyup.enter="submitC"/>

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
      TextCollection: [], // テキストを1文字ずつばらばらに入れて、文字サイズを大小させる {text: 'あ', size: 10, finished: false} のようなかたちにする
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

      // 最後の文字が大きい
      // this.ctx.font = '20px Georgia'
      // let notAnimationCharacter = this.currentBindText.slice(0, -1)
      // let AnimationCharacter = this.currentBindText.slice(-1)
      // this.ctx.fillText(notAnimationCharacter, 50, 50)
      // this.ctx.font = '50px Georgia'
      // this.ctx.fillText(AnimationCharacter, 50, 150)
      // this.ctx.font = '20px Georgia'

      // this.runB()
      this.runC()
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
    runC () {
      // 目的地まで移動する文字 問題点としては、文字ごとに横幅が違うのでfontsizeが大きくなると不自然
      this.TextCollection.forEach((obj, index) => {
        // console.log(obj.sSize)
        // '"Fascinate Inline", cursive, Futura, YuGothic, sans-serif';
        this.ctx.font = obj.size + 'px Fascinate Inline, cursive, Futura, YuGothic, sans-serif'
        this.ctx.fillStyle = obj.color
        obj.cPositionX = obj.cPositionX + (obj.ePositionX - obj.cPositionX) * 0.08
        obj.cPositionY = obj.cPositionY + (obj.ePositionY - obj.cPositionY) * 0.08
        this.ctx.fillText(obj.text, obj.cPositionX, obj.cPositionY)

        let rate = Math.sqrt((obj.ePositionX - obj.cPositionX) * (obj.ePositionX - obj.cPositionX) + (obj.ePositionY - obj.cPositionY) * (obj.ePositionY - obj.cPositionY)) / Math.sqrt((obj.ePositionX - obj.sPositionX) * (obj.ePositionX - obj.sPositionX) + (obj.ePositionY - obj.sPositionY) * (obj.ePositionY - obj.sPositionY)) // 接近率
        console.log(rate)

        if (rate < 0.0001) {

        }
      })
    },
    runB () {
      // 目的地まで移動する文字 問題点としては、文字ごとに横幅が違うのでfontsizeが大きくなると不自然
      this.TextCollection.forEach((obj, index) => {
        // console.log(obj.sSize)
        this.ctx.font = obj.size + 'px Georgia'
        obj.cPositionX = obj.cPositionX + (obj.ePositionX - obj.cPositionX) * 0.5
        obj.cPositionY = obj.cPositionY + (obj.ePositionY - obj.cPositionY) * 0.5
        this.ctx.fillText(obj.text, obj.cPositionX, obj.cPositionY)
        // let rate = // 接近率
      })
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
    },
    bindSubmit (e) {
      this.currentBindText = e.target.value
      console.log(this.currentBindText)
      this.TextCollection = []

      this.currentBindText.split('').forEach((character, index) => {
        // 最後の文字は、animation可能にする
        let sx = 50 + Math.random() * (this.canvas.width - 100)
        let sy = 50 + Math.random() * (this.canvas.height - 100)

        // let ex = 50 + Math.random() * (this.canvas.width - 100)
        // let ey = 50 + Math.random() * (this.canvas.height - 100)

        let width = 25

        if (character.match(/^[^\x01-\x7E\xA1-\xDF]+$/)) { // 全角文字
          width *= 2
        }

        let ex = 50 + index * width
        let ey = 50

        this.TextCollection.push({
          text: character,
          size: 50,
          sPositionX: sx, // 初期x座標
          sPositionY: sy, // 初期y座標
          ePositionX: ex, // 最終x座標
          ePositionY: ey, // 最終x座標
          cPositionX: sx, // 現在x座標
          cPositionY: sy, // 現在y座標
          animationFinshed: false
        })
      })
    },
    updateText (e) {
      this.currentInputText = e.target.value
    },
    submitC (e) {
      // 1文字だけ取得して、真ん中に大きく表示。
      this.currentBindText = e.target.value
      this.TextCollection = []

      // 10回ループ。あとで直す
      this.currentBindText.split('').forEach((character, index) => {
        // 最後の文字は、animation可能にする

        for (var i = 0; i < 3; i++) {
          let sx = 50 + Math.random() * (this.canvas.width - 100)
          let sy = 50 + Math.random() * (this.canvas.height - 100)

          console.log(sx)
          // let ex = 50 + Math.random() * (this.canvas.width - 100)
          // let ey = 50 + Math.random() * (this.canvas.height - 100)

          let ex = 50 + index * 50
          let ey = 100
          this.TextCollection.push({
            text: character,
            size: 50,
            sPositionX: sx, // 初期x座標
            sPositionY: sy, // 初期y座標
            ePositionX: ex, // 最終x座標
            ePositionY: ey, // 最終x座標
            cPositionX: sx, // 現在x座標
            cPositionY: sy, // 現在y座標
            color: '#' + ((1 << 24) * Math.random() | 0).toString(16)
          })
        }
      })
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
