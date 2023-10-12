<!-- eslint-disable vue/multi-word-component-names -->
<script lang="js" setup>
import Kamien from './Kamien.vue'

defineProps({
  plansza: {
    type: Array,
    required: true
  }
})
</script>

<template>
  <div class="plansza">
    <h1>Pędzące żółwie</h1>
    <div class="plansza__container">
      <img src="../assets/plansza.jpg" alt="plansza gry" />
      <Kamien
        v-for="(kamien, index) in plansza"
        :key="index"
        :zolwie="kamien.Zolwie"
        :kamien-index="index"
      />
    </div>
  </div>
</template>

<style lang="scss" scoped>
.plansza {
  display: flex;
  width: 100%;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  &__container {
    position: relative;
    width: fit-content;
  }
}
</style>
