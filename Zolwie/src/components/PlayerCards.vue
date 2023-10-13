<template>
  <div class="players-box ">
    <div class="player" v-for="player in gracze" :key="player.TwojKolor">
      <div v-if="player.TwojeKarty">
        <h2 :class="player.KolorGracza" :style="{ color: player.KolorGracza }">
          {{ player.NazwaGracza }}
        </h2>
        <CardDisplay :cards="player.TwojeKarty" :colorCard="colorMap" />
      </div>
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
  methods: {
    getBorderColor(card) {
      const colorDescriptions = {
        Y: 'yellow',
        B: 'blue',
        G: 'green',
        P: 'purple',
        L: 'white'
      }
      const firstLetter = card[0]
      return colorDescriptions[firstLetter] || 'red'
    }
  },
  data() {
    return {
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
  bottom: 0;
  position: absolute;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
  width: 100%;
  margin-top: 15px;
  padding: 10px;
}

.player {
  width: fit-content;
}

h2 {
  font-weight: 900;
}

.PURPLE {
  color: violet !important;
}
</style>
