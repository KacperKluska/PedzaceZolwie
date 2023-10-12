<!-- eslint-disable vue/multi-word-component-names -->
<script lang="js" setup>
import Kamien from './Kamien.vue'
import data from '../assets/response.json'
</script>

<template>
  <div class="plansza">
    <h1>Pędzące żółwie</h1>
    <div class="plansza__container">
      <img src="../assets/plansza.jpg" alt="plansza gry" />
      <Kamien style="top: 140px; left: 25px" :zolwie="data.PrzebiegGry[3].Plansza[0].Zolwie" />
      <Kamien style="top: 70px; left: 60px" />
      <Kamien style="top: 50px; left: 120px" />
      <Kamien style="top: 60px; left: 180px" />
      <Kamien style="top: 120px; left: 225px" />
      <Kamien style="top: 140px; left: 280px" />
      <Kamien style="top: 100px; left: 330px" />
      <Kamien style="top: 40px; left: 380px" />
      <Kamien style="top: 70px; left: 435px" />
      <Kamien style="top: 110px; left: 485px" />
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
