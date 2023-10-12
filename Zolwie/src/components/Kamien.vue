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
  YELLOW: '#ffff00',
  RED: '#ff0000',
  GREEN: '#00ff00',
  BLUE: '#0000ff',
  PURPLE: '#800080'
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
    left: 25px;
  }
  &-1 {
    top: 70px;
    left: 60px;
  }
  &-2 {
    top: 50px;
    left: 120px;
  }
  &-3 {
    top: 60px;
    left: 180px;
  }
  &-4 {
    top: 120px;
    left: 225px;
  }
  &-5 {
    top: 140px;
    left: 280px;
  }
  &-6 {
    top: 100px;
    left: 330px;
  }
  &-7 {
    top: 40px;
    left: 380px;
  }
  &-8 {
    top: 70px;
    left: 435px;
  }
  &-9 {
    top: 110px;
    left: 485px;
  }
}
</style>
