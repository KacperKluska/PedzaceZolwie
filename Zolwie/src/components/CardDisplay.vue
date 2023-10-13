<template>
  <div class="card-display" v-if="cards">
    <div v-for="card in cards" :key="card" class="card">
      <img class="img" v-if="isSpecialCard(card)" :src="specialCardImage" alt="Special Card" />
      <div class="box-card-mark">
        <span class="card-mark">{{ card }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import dupeczkaImage from '@/assets/dupeczkaRed.png'

export default {
  props: {
    cards: Array,
    colorCard: Object,
    borderColor: String // Dodaj prop borderColor
  },
  data() {
    return {
      specialCardImage: dupeczkaImage
    }
  },
  methods: {

    getColorForCard(card) {
      const firstLetter = card[0]
      return this.colorCard[firstLetter] || 'red'
    },
    getBorderColor(card) {
      // Rozszyfrowywanie koloru
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
}

.card-display {
  display: flex;
  flex-direction: row;
}

.card-mark {
  display: flex;
  justify-content: center;
  width: 30px;
  height: fit-content;
  font-size: 20px;
  background-color: rgba(114, 109, 109, 0.658);
}
</style>
