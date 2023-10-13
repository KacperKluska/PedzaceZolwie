<script setup>
import Plansza from './components/Plansza.vue'
import PlayerCards from './components/PlayerCards.vue';
import CardDisplay from './components/CardDisplay.vue';
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
    //plansza update
    plansza.value = gameStat.PrzebiegGry[runda].Plansza
    //karty gracza update
    let gracz = gracze.value.find(
      (gracz) => gracz.NazwaGracza === gameStat.PrzebiegGry[runda].NazwaGracza
    )
    gracz.TwojeKarty = gameStat.PrzebiegGry[runda].TwojeKarty
    gracz.ZagraneKatry = gameStat.PrzebiegGry[runda].ZagraneKatry

    kartaStos.value = gracz.ZagraneKatry.slice(-1).ZagranaKarta

    console.log(gameStat.PrzebiegGry[runda].ZagraneKatry)
    console.log(kartaStos.value)
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
    <CardDisplay :cards="kartaStos" :colorCard="colorMap" />
  </div>
</template>

<style scoped></style>
