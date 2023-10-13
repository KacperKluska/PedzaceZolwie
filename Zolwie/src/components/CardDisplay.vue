<template>
  <div class="card-display">
    <div
      v-for="(card, index) in cards"
      :key="index"
      class="card"
      :style="{ borderColor: getBorderColor(card) }"
    >
      <img class="img" v-if="card" :src="isSpecialCard(card)" alt="Special Card" />
      <div class="box-card-mark">
        <span class="card-mark">{{ transformCard(card) }}</span>
      </div>
    </div>
  </div>
</template>
<script>
import R from '../assets/karty/R.png'
import B from '../assets/karty/B.png'
import Y from '../assets/karty/Y.png'
import P from '../assets/karty/P.png'
import G from '../assets/karty/G.png'
import Rainbow from '../assets/karty/Rainbow.png'

export default {
  props: {
    cards: Array,
    colorCard: Object
  },
  data() {
    return {
      Red: R,
      Blue: B,
      Yellow: Y,
      Purple: P,
      Green: G,
      Rainbow: Rainbow
    }
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
    },

    isSpecialCard(card) {
      if (card === 'R1') return this.Red
      if (card === 'R2') return this.Red
      if (card === 'R1B') return this.Red
      if (card === 'B1') return this.Blue
      if (card === 'B2') return this.Blue
      if (card === 'B1B') return this.Blue
      if (card === 'G1') return this.Green
      if (card === 'G2') return this.Green
      if (card === 'G1B') return this.Green
      if (card === 'Y1') return this.Yellow
      if (card === 'Y2') return this.Yellow
      if (card === 'Y1B') return this.Yellow
      if (card === 'P1') return this.Purple
      if (card === 'P2') return this.Purple
      if (card === 'P1B') return this.Purple
      if (card === 'L1') return this.Rainbow
      if (card === 'L2') return this.Rainbow
      if (card === 'A1') return this.Rainbow
      if (card === 'A1B') return this.Rainbow
    },

    transformCard(card) {
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
        A1B: '-'
      }
      const regexPattern = Object.keys(cardDescriptions).join('|')
      const regex = new RegExp(`\\b(${regexPattern})\\b`, 'g')
      return card.replace(regex, (match) => cardDescriptions[match])
    }
  },

  computed: {
    specialCardImage() {
      return this.isSpecialCard(this.card)
    }
  }
}
</script>

<style scoped>
.img {
  width: calc(260px / 3);
  height: calc(426px / 3);
}

.card {
  justify-content: flex-end;
  display: flex;
  width: calc(260px / 3);
  height: calc(426px / 3);
  border: 2px solid;
  overflow: hidden;
}

.card-display {
  display: flex;
  flex-direction: row;
}

.box-card-mark {
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 30px;
  height: 30px;
  font-size: 20px;
  background-color: rgba(114, 109, 109, 0.658);
  z-index: 1;
}

.card-mark {
  color: white;
  position: absolute;
}
</style>
