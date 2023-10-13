<script setup>
import Plansza from './components/Plansza.vue'
import PlayerCards from './components/PlayerCards.vue';
import CardOne from './components/CardOne.vue';
import { ref, onMounted } from 'vue'
import gameStat from './assets/response2.json'
let plansza = ref([])
let gracze = ref([])
let runda = 0
let winner = ref()
let kartaStos = ref()

// Funkcja wyświetlająca kolejną tablicę Plansza
const statrGame = () => {
  if (runda < gameStat.PrzebiegGry.length) {
    kartaStos.value = undefined
    //plansza update
    plansza.value = gameStat.PrzebiegGry[runda].Plansza
    //karty gracza update
    let gracz = gracze.value.find(
      (gracz) => gracz.NazwaGracza === gameStat.PrzebiegGry[runda].NazwaGracza
    )
    gracz.TwojeKarty = gameStat.PrzebiegGry[runda].TwojeKarty
    gracz.ZagraneKatry = gameStat.PrzebiegGry[runda].ZagraneKatry

    // kartaStos.value = gameStat.PrzebiegGry[runda].ZagraneKatry.slice(-1)[0] ? gameStat.PrzebiegGry[runda].ZagraneKatry.slice(-1)[0].ZagranaKarta : ""
    kartaStos.value = gameStat.PrzebiegGry[runda].ZagraneKatry
    console.log("----", kartaStos.value)
    runda += 1
  } else {
    runda = 0
    winner.value = gameStat.WynikGry.WygranyGracz
    clearInterval(interval)
  }
}

const colorMap = {
  R: 'red',
  G: 'green',
  B: 'blue',
  Y: 'yellow',
  P: 'purple',
  L: 'white'
}

onMounted(() => {
  gracze.value = gameStat.NowaGra.Gracze
  for (let i = 0; i < gameStat.NowaGra.Gracze.length; i++) {

    gracze[i].TwojeKarty = []
  }

})

// Wywołanie funkcji co x sekund
const interval = setInterval(statrGame, 1500)
</script>

<template>
  <div>
    <Plansza :plansza="plansza" />
    <PlayerCards v-if="gracze.length" :gracze="gracze" />
    <div v-for="(card, index) in kartaStos" :key="index">
      <p>{{ card.ZagranaKarta }}</p>
      <CardOne v-if="kartaStos" class="karta-stos" :card="card.ZagranaKarta" :colorCard="colorMap" />
    </div>
  </div>
</template>

<style>
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

body {
  background-color: black !important;
}

#app {
  height: 100vh;
  position: relative;
}

.karta-stos {
  /* border: 5px solid white;
  border-radius: 5px;
  overflow: hidden;
  width: calc(260px / 3);
  height: calc(426px / 3); */
  width: fit-content;
  position: absolute;
  left: 47%;
  transform: scale(1.3);
  top: 401px;
  animation: exampleA 500ms;
}
</style>
