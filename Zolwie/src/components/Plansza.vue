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
      <Kamien v-for="(kamien, index) in plansza" :key="index" :zolwie="kamien.Zolwie" :kamien-index="index" />
    </div>
  </div>
</template>

<style lang="scss" scoped>
@keyframes exampleB {
  0% {
    transform: perspective(20px) rotateX(30deg);
  }

  100% {
    transform: perspective(200px) rotateX(30deg);
  }
}

.plansza {
  display: flex;
  width: 100%;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  &__container {
    transform: scale(1.5);
    position: relative;
    width: fit-content;
  }

  img {
    transform: perspective(200px) rotateX(30deg);
    animation: exampleB 2000ms;
  }
}
</style>
