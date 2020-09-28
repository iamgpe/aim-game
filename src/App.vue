<template>
  <div id="app">
    <div class="home" v-if="!started">
      <div class="middle">
        <h1>Aim Game</h1>
        <span v-if="loosed">Perdu ! Le score obtenu est de {{ score }}.</span>
        <button @click="start">Commencer le niveau {{ level }}</button>
      </div>
    </div>
    <div class="game" v-else>
      <div class="plate">
        <circle-tap 
          v-for="(circle, i) in circlesShowed" 
          :key="i"
          :circle="circle"
        ></circle-tap>
      </div>
      <div class="down">
        <h1>AimGame</h1>
        <span>🏆 {{ score }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  computed: {
    circlesShowed () {
      return this.circles.filter(circle => !circle.taped)
    }
  },
  data: () => ({
    circles: [],
    loosed: false,
    started: false,
    level: 1,
    score: 0,
  }),
  mounted () {
    this.$root.$on('circle:tap', id => {
      this.removeCircle(id)
      this.stopIfAllCircleAreTaped()

      this.score++
    })
    this.$root.$on('game over', () => this.gameOver())
  },
  methods: {
    start () {
      this.score = this.level == 1 ? 0 : this.score
      this.started = true
      this.loosed = false
      this.circles = []

      this.generateRandomCircles(5, this.level)
    },
    gameOver () {
      this.loosed = true
      this.started = false
      this.level = 1
    },
    generateRandomCircles (number, level) {
      for (let i = 1; i <= number; i++) {
        this.circles.push({
          id: i,
          taped: false,
          left: (Math.random() * (document.body.clientWidth - 100)).toFixed(),
          top: (Math.random() * (document.body.clientHeight - 80 - 100)).toFixed(),
          level: this.level,
        })
      }
    },
    stopIfAllCircleAreTaped () {
      if (this.circles.filter(circle => !circle.taped).length < 1) {
        this.level++
        this.started = false
        this.loosed = false
      }
    },
    removeCircle (id) {
      this.circles = this.circles.map(circle => {
        if (circle.id == id) {
          circle.taped = true
        }

        return circle
      })
    },
  }
}
</script>

<style lang="scss">
  * {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #fff;
    width: 100%;
    height: 100vh;
    background-color: #212121;

    > .home {
      position: fixed;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      text-align: center;
      display: flex;
      align-items: center;
      justify-content: center;

      > .middle {
        > h1, 
        > span {
          float: left;
          clear: both;
          width: 100%;
          text-align: center;
        }

        > span {
          margin-top: 15px;
        }

        > button {
          border: none;
          background-color: #fff;
          padding: 0 10px;
          line-height: 30px;
          text-transform: uppercase;
          height: 30px;
          font-weight: 600;
          margin-top: 15px;
          cursor: pointer;
          border-radius: 5px;
          color: #212121;
        }
      }
    }

    > .game {
      position: fixed;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;

      > .plate {
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 50px;
      }

      > .down {
        position: fixed;
        bottom: 0;
        width: 100%;
        height: 50px;
        background-color: rgba(#fff, 0.1);
        padding: 0 18px;

        > h1 {
          float: left;
          line-height: 50px;
          font-size: 18px;
          font-weight: 500;
        }

        > span {
          float: right;
          line-height: 50px;
        }
      }
    }
  }
</style>
