<script setup>
import Plansza from './components/Plansza.vue'
import PlayerCards from './components/PlayerCards.vue';
import { ref, onMounted } from 'vue'
import gameStat from './assets/response2.json'
let plansza = ref([])
let gracze = ref([])
let runda = 0
let winner = ref()

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
    runda += 1
  } else {
    runda = 0
    winner.value = gameStat.WynikGry.WygranyGracz
    clearInterval(interval)
  }
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
    <br />
    <p>{{ plansza }}</p>
    <br />
    <PlayerCards v-if="gracze.length" :gracze="gracze" />
    <p>{{ gracze }}</p>
    <br />
    <p v-if="winner">{{ winner }}</p>
  </div>
</template>

<style scoped></style>
