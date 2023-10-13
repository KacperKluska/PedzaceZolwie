<template>
  <div class="card-display">
    <p>{{ cards }}</p>
    <div v-for="(card, index) in cards" :key="index" class="card" :style="{ borderColor: getBorderColor(card) }">
      <img class="img" v-if="isSpecialCard(card)" :src="specialCardImage" alt="Special Card" />
      <div class="box-card-mark">
        <span class="card-mark">{{ transformCard(card) }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import dupeczkaImage from '@/assets/dupeczkaRed.png'

export default {
  props: {
    cards: Array,
    colorCard: Object
  },
  data() {
    return {
      specialCardImage: dupeczkaImage
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
        A2: '++'
      }
      const regexPattern = Object.keys(cardDescriptions).join('|')
      const regex = new RegExp(`\\b(${regexPattern})\\b`, 'g')
      return card.replace(regex, (match) => cardDescriptions[match])
    },
    isSpecialCard(card) {
      return card === 'R1'
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
