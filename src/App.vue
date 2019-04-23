<template>
  <div id="app">
    <grid v-if="data" :data="data" ref="grid" @finished="restart"></grid>
    <transition name="fade">
      <loader v-if="loader"></loader>
    </transition>
    <transition name="fade">
      <button v-if="button" class="button" @click="start">{{ button }}</button>
    </transition>
  </div>
</template>

<script>
import './assets/styles.sass'
import Loader from './components/Loader.vue'
import Grid from './components/Grid'
import axios from 'axios'
import swal from 'sweetalert2'

export default {
  name: 'app',
  components: {
    Loader,
    Grid
  },
  data () {
    return {
      data: null,
      loader: true,
      button: false
    }
  },
  created () {
    axios.get('/bests.json').then((response) => {
      this.data = response.data
      setTimeout(() => {
        this.loader = false
        this.button = 'Start'
      }, 500)
    }).catch((error) => {
      if (error) {
        swal.fire({
          type: 'error',
          title: 'Oops...',
          text: 'Something went wrong!'
        })
      }
    })
  },
  methods: {
    start () {
      if (this.$refs['grid'] === undefined) {
        swal.fire({
          type: 'error',
          title: 'Oops...',
          text: 'Something went wrong!'
        })
        return
      }
      this.button = false
      setTimeout(() => {
        this.$refs['grid'].start()
      }, 500)
    },
    restart () {
      this.button = 'Restart'
    }
  }
}
</script>
