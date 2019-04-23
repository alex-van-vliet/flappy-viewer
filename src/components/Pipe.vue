<template>
  <div class="pipe" :style="{ left: (pos + 2 - x) * 100 / width + '%', width : 300 / width + '%' }">
    <div class="pipe-top" :style="{ height: top * 100 / height  + '%' }"></div>
    <div class="pipe-bottom" :style="{ height: bottom * 100 / height + '%' }"></div>
  </div>
</template>

<script>
export default {
  name: 'Pipe',
  props: ['pos', 'top', 'free', 'bottom', 'x', 'width', 'height']
}
</script>
