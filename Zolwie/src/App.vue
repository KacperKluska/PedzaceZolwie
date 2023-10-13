<script setup>
import Plansza from './components/Plansza.vue'
import { ref, onMounted } from 'vue'
import gameStat from './assets/response2.json'
// import KartaGry from './components/KartaGry.vue'
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

const findWinnerColor = (name) => {
  const znalezionyZwyciezca = gameStat.NowaGra.Gracze.find(
    (druzyna) => druzyna.NazwaGracza === name
  )
  console.log('🚀 ~ file: App.vue:38 ~ findWinnerColor ~ znalezionyZwyciezca:', znalezionyZwyciezca)
  return znalezionyZwyciezca.KolorGracza || undefined
}

// Wywołanie funkcji co x sekund
const interval = setInterval(statrGame, 100)
</script>

<template>
  <div>
    <Plansza :plansza="plansza" />
    <div class="wygrana" v-if="winner">
      The Winer is...<br />
      {{ winner }}
      <div class="wygrana__images">
        <img
          class="wygrana__zolw"
          :src="`src/assets/zolwie/${findWinnerColor(winner).toLowerCase()}.png`"
          alt="zwycięzki żółw"
        />
        <img class="wygrana__winer" src="./assets/winer.png" alt="Winer" />
      </div>
    </div>
    <br />
    <p>{{ plansza }}</p>
    <!-- <KartaGry :color="'czerwony'" :option="'+'" /> -->
    <br />
    <p>{{ gracze }}</p>
    <br />
    <p v-if="winner">{{ winner }}</p>
  </div>
</template>

<style lang="scss" scoped>
@keyframes winner {
  0% {
    left: -1000px;
  }
  100% {
    left: 50%;
  }
}
@keyframes spinningTurtle {
  0% {
    left: -1000px;
    top: 500px;
    transform: translate(-50%, 0%) scale(0.1) rotateZ(0) rotateY(180deg);
  }
  100% {
    left: 50%;
    top: 300px;
    transform: translate(-50%, 0%) scale(2) rotateZ(720deg) rotateY(180deg);
  }
}
.wygrana {
  position: absolute;
  font-size: 60px;
  line-height: 60px;
  width: 100%;
  color: yellow;
  text-align: center;
  font-weight: 900;
  left: 50%;
  top: 0;
  transform: translate(-50%, 0%);
  animation: winner 1000ms;

  &__images {
    position: absolute;
    left: 50%;
    top: 300px;
    transform: translate(-50%, 0%) rotateY(180deg) scale(2);
    animation: spinningTurtle 3000ms;
  }

  &__winer {
    position: absolute;
    left: -23px;
    top: -28px;
    scale: 1.3;
    transform: rotateY(180deg) rotateZ(10deg);
  }

  &__zolw {
    width: 203px;
    height: 131px;
  }
}
</style>
