<!-- eslint-disable vue/multi-word-component-names -->
<script lang="js" setup>
const props = defineProps({
  color: {
    type: String,
    required: true
  }
})
</script>

<template>
  <img
    class="zolw"
    :src="`src/assets/zolwie/${color.toLowerCase()}.png`"
    :alt="`${color} turtle`"
  />
</template>

<style lang="scss" scoped>
@keyframes exampleA {
  0% {
    scale: 2;
    opacity: 0.1;
  }
  100% {
    scale: 1;
    opacity: 1;
  }
}

.zolw {
  width: 50px;
  height: 50px;
  position: absolute;
  transform: rotateY(180deg);
  animation-name: exampleA;
  animation-duration: 1.5s;
}
</style>
