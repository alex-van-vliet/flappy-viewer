<template>
  <div @click="toggle" v-if="enabled" :class="{ fullscreen: true, disable: disable }">
    <div class="arrow-main"></div>
    <div class="arrow-secondary"></div>
  </div>
</template>

<script>
import screenfull from 'screenfull'

export default {
  name: 'Fullscreen',
  methods: {
    toggle () {
      screenfull.toggle()
    }
  },
  computed: {
    enabled () {
      return screenfull.enabled
    }
  },
  data () {
    return {
      disable: false
    }
  },
  created () {
    this.disable = screenfull.isFullscreen
    if (screenfull.enabled) {
      screenfull.on('change', () => {
        this.disable = screenfull.isFullscreen
      })
    }
  }
}
</script>

<style scoped>

</style>
