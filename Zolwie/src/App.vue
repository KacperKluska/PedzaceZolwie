<script setup>
import Plansza from './components/Plansza.vue'
import PlayerCards from './components/PlayerCards.vue'
import CardOne from './components/CardOne.vue'
import { ref } from 'vue'
// import response from './assets/response2.json'
import response from './assets/json.json'
let counter = 1000
let plansza = ref([])
let gracze = ref([])
let runda = 0
let winner = ref()
let kartaStos = ref()
let gameStat = ref()
let gameStarted = ref(false)
let nextButtonEnable = ref(false)
let gameNumber = ref(0)

var intervalID = null

function intervalManager(flag, animate) {
  if (flag) intervalID = setInterval(animate, counter)
  else clearInterval(intervalID)
}

// Funkcja wyświetlająca kolejną tablicę Plansza
const statrGame = () => {
  if (runda < gameStat.value.PrzebiegGry.length) {
    kartaStos.value = undefined
    //plansza update
    plansza.value = gameStat.value.PrzebiegGry[runda].Plansza
    //karty gracza update
    let gracz = gracze.value.find(
      (gracz) => gracz.NazwaGracza === gameStat.value.PrzebiegGry[runda].NazwaGracza
    )
    gracz.TwojeKarty = gameStat.value.PrzebiegGry[runda].TwojeKarty
    gracz.ZagraneKatry = gameStat.value.PrzebiegGry[runda].ZagraneKatry

    kartaStos.value = gameStat.value.PrzebiegGry[runda].ZagraneKatry
    console.log('----', kartaStos.value)
    runda += 1
  } else {
    runda = 0
    winner.value = gameStat.value.WynikGry.WygranyGracz
    intervalManager(false) // for clearInterval
    // clearInterval(interval)
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

const nextRound = () => {
  console.log('aaaaaaaa', gameNumber.value)
  plansza.value = []
  gracze.value = []
  gameStarted.value = false
  winner.value = undefined
  kartaStos.value = undefined
  gameStat.value = undefined
  gameNumber.value += 1
  nextButtonEnable.value = true
}

const startGame = () => {
  intervalManager(true, statrGame) // for setInterval
  // interval.value = setInterval(statrGame, counter.value)
  gameStarted.value = true
  gameStat.value = response[gameNumber.value]
  gracze.value = gameStat.value.NowaGra.Gracze
  for (let i = 0; i < gameStat.value.NowaGra.Gracze.length; i++) {
    gracze[i].TwojeKarty = []
  }
}

// const interval = setInterval(statrGame, counter.value)
const findWinnerColor = (name) => {
  const znalezionyZwyciezca = gameStat.value.NowaGra.Gracze.find(
    (druzyna) => druzyna.NazwaGracza === name
  )
  return znalezionyZwyciezca.KolorGracza || undefined
}

// Wywołanie funkcji co x sekund
</script>

<template>
  <div class="error" v-if="winner === ''">
    ERROR 500: Po stronie serwera wystąpił Robaszek
    <br />
    <img src="./assets/robaszek.png" alt="robaszek" />
  </div>
  <div v-else>
    <div v-if="!gameStarted" class="btn" @click="startGame">
      <button>Start</button>
    </div>
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
      <div class="btn wygrana-btn" @click="nextRound">
        <button>Next round</button>
      </div>
    </div>
    <div v-if="gameStarted" class="gra" :class="{ 'gra--koniec': winner }">
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
@keyframes winner-btn {
  0% {
    opacity: 0;
  }

  70% {
    opacity: 0;
  }

  100% {
    opacity: 1;
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

@keyframes robaszek {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(90deg);
  }
}

.error {
  width: 100%;
  height: 100vh;
  font-size: 40px;
  font-weight: 700;
  color: red;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;

  img {
    animation: robaszek 2s ease-in;
    animation-fill-mode: forwards;
  }
}

.gra {
  position: relative;
  top: 0;
  height: 100vh;
  transition: top 1s;
  padding-top: 50px;

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

  &-btn {
    animation: winner-btn 4000ms ease-in-out;
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
  margin-top: 40px;
  display: flex;
  justify-content: center;
}
</style>
