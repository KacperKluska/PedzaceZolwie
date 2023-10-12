<!-- eslint-disable vue/multi-word-component-names -->
<script lang="ts" setup>
import Zolw from './Zolw.vue'

defineProps({
  zolwie: {
    type: Array,
    required: true
  },
  kamienIndex: {
    type: Number,
    required: true
  }
})

const colors = {
  YELLOW: 'yellow',
  RED: 'red',
  GREEN: 'green',
  BLUE: 'blue',
  PURPLE: 'purple'
}
</script>

<template>
  <div class="kamien" :class="`kamien-${kamienIndex}`">
    <Zolw
      v-for="(zolw, index) in zolwie"
      :style="{ left: '0px', bottom: `${index * 15}px` }"
      :color="colors[`${zolw}`]"
      :key="index"
    />
  </div>
</template>

<style lang="scss" scoped>
.kamien {
  position: absolute;
  width: 50px;
  height: 100px;

  &-0 {
    top: 140px;
    left: -15px;
  }
  &-1 {
    top: 60px;
    left: 60px;
  }
  &-2 {
    top: 40px;
    left: 125px;
  }
  &-3 {
    top: 50px;
    left: 180px;
  }
  &-4 {
    top: 120px;
    left: 220px;
  }
  &-5 {
    top: 140px;
    left: 285px;
  }
  &-6 {
    top: 90px;
    left: 335px;
  }
  &-7 {
    top: 30px;
    left: 380px;
  }
  &-8 {
    top: 50px;
    left: 435px;
  }
  &-9 {
    top: 100px;
    left: 510px;
  }
}
</style>
