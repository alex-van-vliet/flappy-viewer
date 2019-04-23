<template>
  <div class="bird-box" :style="{ width: 100 / width + '%', height: 100 / height + '%', left: 200 / width + '%', top: y * 100 / height + '%' }">
    <div class="bird" :style="{ backgroundColor: color }">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Bird',
  props: ['color', 'y', 'width', 'height']
}
</script>
