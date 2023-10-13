<template>
  <div class="players-box">
    <div class="player" v-for="player in gracze" :key="player.TwojKolor">
      <h2 style="color: aliceblue">Gracz {{ player.NazwaGracza }}</h2>
      <CardDisplay :cards="player.TwojeKarty" :colorCard="colorMap" />
    </div>
  </div>
</template>

<script>
import CardDisplay from './CardDisplay.vue'

export default {
  props: {
    gracze: Array
  },
  components: {
    CardDisplay
  },
  data() {
    return {
      players: [
        {
          id: 1,
          cards: ['G1B', 'R1B', 'A1', 'G1', 'B2']
        },
        {
          id: 2,
          cards: ['R1', 'R1', 'Y1B', 'P1B', 'B1']
        }
      ],
      colorMap: {
        R: 'red',
        G: 'green',
        B: 'blue',
        Y: 'yellow',
        P: 'purple',
        L: 'white'
      }
    }
  }
}
</script>

<style>
.players-box {
  width: 100%;
  padding: 10px;
  display: flex;
  gap: 20px;
}

.player {
  width: fit-content;
}
</style>
