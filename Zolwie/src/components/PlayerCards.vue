<template>
  <div class="players-box" v-if="gracze">
    <p>{{ gracze }}</p>
    <div class="player" v-for="player in gracze" :key="player.TwojKolor">
      <h2 style="color: #f0f8ff">Gracz {{ player.NazwaGracza }}</h2>
      <h2 style="color: #f0f8ff">TwojeKarty {{ player.TwojeKarty }}</h2>
      <CardDisplay v-if="player.TwojeKarty.length" :cards="transformCards(player.TwojeKarty)" :colorCard="colorMap" />
    </div>
  </div>
</template>

<script>
import CardDisplay from './CardDisplay.vue'

function assignColorToCard(card) {
  const colorDescriptions = {
    Y1: 'yellow',
    G2: 'green',
    P2: 'purple',
    L1: 'white',
    R1: 'red'
  }

  const regexPattern = Object.keys(colorDescriptions).join('|')
  const regex = new RegExp(`\\b(${regexPattern})\\b`, 'g')

  return card.replace(regex, (match) => colorDescriptions[match])
}

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
  },
  methods: {
    transformCards(cards) {
      const cardDescriptions = {
        R1: '+',
        R2: '++',
        B1: '+',
        B2: '++',
        G1: '+',
        G2: '++',
        Y1: '+',
        Y2: '++',
        P1: '+',
        P2: '++',
        R1B: '-',
        Y1B: '-',
        G1B: '-',
        B1B: '-',
        P1B: '-',
        L1: 'Up',
        L2: 'Up x2',
        A1: '+',
        A2: '++'
      }

      const regexPattern = Object.keys(cardDescriptions).join('|')
      const regex = new RegExp(`\\b(${regexPattern})\\b`, 'g')

      return cards.map((card) => {
        return assignColorToCard(card.replace(regex, (match) => cardDescriptions[match]))
      })
    }
  }
}
</script>
