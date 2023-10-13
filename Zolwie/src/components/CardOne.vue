<template>
    <div class="card-display">
        <div class="card">
            <img class="img" v-if="card" :src="isSpecialCard(card)" alt="Special Card" />
            <div class="box-card-mark">
                <!-- <img class="card-mark" :src="transformCard(card)" alt=""> -->
                <img class="card-mark" :src="transformCard(card)" alt="">
                <!-- <span class="card-mark">{{ transformCard(card) }}</span> -->
            </div>
            <div class="box-card-mark-left">
                <!-- <img class="card-mark" :src="transformCard(card)" alt=""> -->
                <img class="card-mark-left" :src="transformCard(card)" alt="">
                <!-- <span class="card-mark">{{ transformCard(card) }}</span> -->
            </div>
            <div class="box-card-mark-center">
                <!-- <img class="card-mark" :src="transformCard(card)" alt=""> -->
                <img class="card-mark-center" :src="transformCard(card)" alt="">
                <!-- <span class="card-mark">{{ transformCard(card) }}</span> -->
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
import plus from '@/assets/karty/plus.png'
import plus2 from '@/assets/karty/plus2.png'
import minus from '@/assets/karty/plus.png'
import Up from '@/assets/karty/Up.png'
import Up2x from '@/assets/karty/Up2x.png'

export default {
    props: {
        card: String,
        colorCard: Object
    },
    data() {
        return {
            Red: R,
            Blue: B,
            Yellow: Y,
            Purple: P,
            Green: G,
            Rainbow: Rainbow,
            plus, plus2, minus, Up, Up2x
            // specialCardImage: dupeczkaImage
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
                R1: plus,
                R2: plus2,
                B1: plus,
                B2: plus2,
                G1: plus,
                G2: plus2,
                Y1: plus,
                Y2: plus2,
                P1: plus,
                P2: plus2,
                R1B: minus,
                Y1B: minus,
                G1B: minus,
                B1B: minus,
                P1B: minus,
                L1: Up,
                L2: Up2x,
                A1: plus,
                A1B: plus2
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
    position: relative;
    justify-content: flex-end;
    display: flex;
    width: calc(260px / 3);
    height: calc(426px / 3);
    border: 2px solid white;
    border-radius: 4px;
    overflow: hidden;
}

.card-display {
    display: flex;
    flex-direction: row;
}

.box-card-mark {
    /* position: absolute; */
    display: flex;
    justify-content: center;
    align-items: center;
    width: 30px;
    z-index: 1;
}

.box-card-mark-left {
    /* position: absolute; */
    display: flex;
    justify-content: center;
    align-items: center;
    width: 30px;
    z-index: 1;
}

.box-card-mark-center {
    /* transform: translate(50%, 50%); */
    /* top: 50%; */
    /* left: 50%; */
    /* position: absolute; */
    display: flex;
    justify-content: center;
    align-items: center;
    width: 50px;
    z-index: 1;
}

.card-mark {
    width: 20px;
    top: 2px;
    right: 2px;
    color: white;
    position: absolute;
}

.card-mark-left {
    width: 20px;
    top: 2px;
    left: 2px;
    color: white;
    position: absolute;
}

.card-mark-center {
    transform: translate(-50%, -50%);
    top: 60%;
    left: 47%;
    width: 45px;
    color: white;
    position: absolute;
}
</style>
  