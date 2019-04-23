<template>
  <div class="name" :class="{ shown: shown }"
       :style="{ top: Math.floor(i / 5) * 18 + 'px', left: (i % 5) * 20 + '%' }" @click="fire">
    <div class="color" :style="{ backgroundColor: color }"></div>
    <div class="text">{{ name }}</div>
  </div>
</template>

<script>
export default {
  name: 'Name',
  props: ['color', 'name', 'i', 'shown'],
  methods: {
    fire () {
      this.$emit('click')
    }
  }
}
</script>
