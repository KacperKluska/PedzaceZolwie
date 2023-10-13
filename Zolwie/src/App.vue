<script setup>
import Plansza from './components/Plansza.vue'
import PlayerCards from './components/PlayerCards.vue'
import CardOne from './components/CardOne.vue'
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
    console.log('----', kartaStos.value)
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

const findWinnerColor = (name) => {
  const znalezionyZwyciezca = gameStat.NowaGra.Gracze.find(
    (druzyna) => druzyna.NazwaGracza === name
  )
  return znalezionyZwyciezca.KolorGracza || undefined
}

// Wywołanie funkcji co x sekund
const interval = setInterval(statrGame, 100)
</script>

<template>
  <div class="wygrana" v-if="winner">
    The Winer is...<br />
    <h1 class="wygrana__zwyciezca">{{ winner }}</h1>
    <div class="wygrana__images">
      <img
        class="wygrana__zolw"
        :src="`src/assets/zolwie/${findWinnerColor(winner).toLowerCase()}.png`"
        alt="zwycięzki żółw"
      />
      <img class="wygrana__winer" src="./assets/winer.png" alt="Winer" />
    </div>
  </div>
  <div class="gra" :class="{ 'gra--koniec': winner }">
    <Plansza :plansza="plansza" />
    <div v-for="(card, index) in kartaStos" :key="index">
      <CardOne
        v-if="kartaStos"
        class="karta-stos"
        :card="card.ZagranaKarta"
        :colorCard="colorMap"
      />
    </div>
    <PlayerCards v-if="gracze.length" :gracze="gracze" />
    <!-- <CardDisplay :cards="kartaStos" :colorCard="colorMap" /> -->
  </div>
</template>

<style lang="scss">
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
  position: relative;
  margin-top: 0;
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
    top: 320px;
    transform: translate(-50%, 0%) scale(2) rotateZ(720deg) rotateY(180deg);
  }
}

.gra {
  position: relative;
  top: 0;
  height: 100vh;
  transition: top 1s;

  &--koniec {
    top: 500px;
  }
}

.wygrana {
  z-index: 10;
  position: absolute;
  font-size: 40px;
  line-height: 40px;
  width: 100%;
  color: yellow;
  text-align: center;
  font-weight: 700;
  left: 50%;
  top: 0;
  transform: translate(-50%, 0%);
  animation: winner 1000ms;

  &__zwyciezca {
    position: relative;
    font-size: 50px;
    line-height: 50px;
    font-weight: 900;
    left: 50%;
    margin-top: 20px;
    transform: translate(-50%, 0%);
    animation: winner 3000ms;
  }

  &__images {
    position: absolute;
    left: 50%;
    top: 320px;
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
