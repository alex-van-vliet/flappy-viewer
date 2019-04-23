<template>
  <div class="grid">
    <pipe v-for="(pipe, i) in shownPipes" :key="'pipe-' + i" :pos="pipe.pos" :top="pipe.top" :free="pipe.free"
          :bottom="pipe.bottom" :x="x" :width="width" :height="height"></pipe>
    <div class="names">
      <name v-for="(bird, i) in birds" @click="toggle(bird)" :key="'name-' + i" :color="bird.color" :name="bird.name"
            :i="i" :shown="shown === 0 || bird.shown"></name>
    </div>
    <bird v-for="(bird, i) in aliveBirds" :key="'bird-' + i" :color="bird.color" :y="bird.positions[x].pos"
          :width="width" :height="height"></bird>
    <fullscreen></fullscreen>
  </div>
</template>

<script>
import Bird from './Bird.vue'
import Pipe from './Pipe.vue'
import Name from './Name.vue'
import Fullscreen from './Fullscreen.vue'

export default {
  name: 'Grid',
  components: {
    Pipe,
    Bird,
    Name,
    Fullscreen
  },
  data () {
    return {
      width: 0,
      height: 0,
      birds: [],
      pipes: [],
      x: 0,
      interval: -1,
      shown: 0
    }
  },
  props: ['data'],
  created () {
    this.width = this.data.width
    this.height = this.data.height
    this.birds = this.data.birds.map((bird) => {
      return { ...bird, shown: false }
    })
    this.pipes = [ ...this.data.pipes ]
  },
  computed: {
    shownPipes () {
      let shown = []
      for (let pipe in this.pipes) {
        if (this.pipes[pipe].pos >= this.x + this.width) {
          break
        }
        shown.push(this.pipes[pipe])
      }
      return shown
    },
    aliveBirds () {
      return this.birds.filter((bird) => {
        return bird.positions.length > this.x && (this.shown === 0 || bird.shown)
      })
    },
    stop () {
      return this.birds.filter((bird) => {
        return bird.positions.length > this.x
      }).length === 0
    }
  },
  methods: {
    removeUselessPipes () {
      for (let i = 0; i < this.pipes.length && this.pipes[i].pos + 4 < this.x; this.pipes.splice(0, 1)) {
      }
    },
    toggle (bird) {
      if (bird.shown) {
        this.shown -= 1
        bird.shown = false
      } else {
        this.shown += 1
        bird.shown = true
      }
    },
    start () {
      if (this.interval !== -1) {
        return
      }
      this.x = 0
      this.pipes = [...this.data.pipes]
      this.interval = setInterval(() => {
        if (this.stop) {
          clearInterval(this.interval)
          this.interval = -1
          this.$emit('finished')
          return
        }
        this.x += 1
        this.removeUselessPipes()
      }, 25)
    }
  }
}
</script>
