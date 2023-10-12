<!-- eslint-disable vue/multi-word-component-names -->
<script lang="ts" setup>
import Zolw from './Zolw.vue'

const props = defineProps({
  zolwie: {
    type: Array,
    required: true
  }
})

console.log('elo', props.zolwie)

const colors = {
  YELLOW: '#ffff00',
  RED: '#ff0000',
  GREEN: '#00ff00',
  BLUE: '#0000ff',
  PURPLE: '#800080'
}
</script>

<template>
  <div class="kamien">
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
}
</style>
