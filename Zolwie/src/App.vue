<script setup>
import Plansza from './components/Plansza.vue'
import { ref, onMounted } from 'vue'
import gameStat from './assets/response.json'
import KartaGry from './components/KartaGry.vue'
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
})

// Wywołanie funkcji co x sekund
const interval = setInterval(statrGame, 1500)
</script>

<template>
  <p>{{ plansza }}</p>
  <KartaGry :color="'czerwony'" :option="'+'" />
  <br />
  <p>{{ gracze }}</p>
  <br />
  <p v-if="winner">{{ winner }}</p>
  <Plansza />
</template>

<style scoped></style>
