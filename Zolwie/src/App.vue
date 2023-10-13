<script setup>
import Plansza from './components/Plansza.vue'
import PlayerCards from './components/PlayerCards.vue'
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
    <div class="btn">
      <button>Start</button>
    </div>
    <Plansza :plansza="plansza" />
    <PlayerCards v-if="gracze.length" :gracze="gracze" />
    <!-- <CardDisplay :cards="kartaStos" :colorCard="colorMap" /> -->
  </div>
</template>

<style scoped>
/* Styl przycisku */
button {
  background: linear-gradient(90deg, #dce31aad, #06eb1da8);
  color: #ffffff;
  border: 2px solid #ffffff;
  text-shadow: 2px 2px 2px rgba(192, 145, 36, 0.5);
  box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.5);
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
  transition:
    background 0.3s,
    color 0.3s;

  border-radius: 15px;
  font-size: 20px;
  padding: 15px 40px 15px 40px;
  font-weight: 700;
}

/* Styl przycisku po najechaniu myszą */
button:hover {
  background: linear-gradient(90deg, #d3770e, #ad931095);
  color: #00ff00;
}

/* Styl przycisku po kliknięciu */
button:active {
  background: linear-gradient(90deg, #000000, #1a1a1a);
  color: #ffffff;
  border: 2px solid #1a1a1a;
}

.btn {
  display: flex;
  justify-content: center;
}
</style>
