<template>
  <div 
    class="circle" 
    @click="tap" 
    :style="`left:${circle.left}px;top:${circle.top}px;animation-duration:${duration}s!important;`"
    :level="circle.level"
  >
    {{ circle.id }}
  </div>
</template>

<script>
export default {
  props: ['circle'],
  data: () => ({
    taped: false,
    duration: 10,
    timeout: null
  }),
  mounted () {
    this.duration = this.$props.circle.level == 1
      ? 10
      : 10 - (10 * (0.25 * (this.$props.circle.level - 1)))

    this.timeout = setTimeout(() => {
      if (!this.taped) {
        this.$root.$emit('game over')
      }
    }, this.duration * 1000);
  },
  methods: {
    tap () {
      this.timeout = clearTimeout(this.timeout)
      
      this.taped = true
      this.timeout = null

      this.$root.$emit('circle:tap', this.$props.circle.id)
    }
  }
}
</script>

<style>
.circle {
  position: fixed;
  width: 100px;
  height: 100px;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 50%;
  line-height: 100px;
  text-align: center;
  animation-fill-mode: forwards;
  animation-name: disappears;
  animation-timing-function: linear;
}

@keyframes disappears {
  from {
    transform: scale(1);
  }

  to {
    transform: scale(0);
  }
}
</style>