<template>
  <div class="players-box">
    <div class="player" v-for="player in players" :key="player.id">
      <h2 style="color: aliceblue">Gracz {{ player.id }}</h2>
      <CardDisplay :cards="player.cards" :colorCard="colorMap" />
    </div>
  </div>
</template>

<script>
import CardDisplay from './CardDisplay.vue'

export default {
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
