<template>
  <div>
    <h1 class="title">
      <img class="vue-logo" src="./assets/logo.png" />
      Card Shuffling
    </h1>
    <div class="speed-buttons">
      <button v-for="(type, index) in shuffleTypes" class="button is-small" :key="index" :class="{ 'is-light': shuffleSpeed != `shuffle${type}` }" @click="shuffleSpeed = `shuffle${type}`">
        {{ type }}
      </button>
    </div>
    <div class="main-buttons">
      <button v-if="isDeckShuffled" @click="displayInitialDeck" class="button is-primary is-outlined">
        Reset <i class="fas fa-undo"></i>
      </button>
      <button @click="shuffleDeck" class="button is-primary">
        Shuffle <i class="fas fa-random"></i>
      </button>
    </div>
    <transition-group :name="shuffleSpeed" tag="div" class="deck">
      <decks v-for="(card, index) in this.cards" :key="index" :card="card" :suitColor="suitColor"></decks>
    </transition-group>
  </div>
</template>

<script>
import Decks from './components/decks.vue';
import Vue from 'vue';

export default {
  components: {
    Decks
  },
  data () {
    return {
      ranks: ['A', '2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K'],
      suits: ['♥', '♦', '♠', '♣'],
      cards: [],
      suitColor: {
        '♠': 'black',
        '♣': 'black',
        '♦': 'red',
        '♥': 'red'
      },
      shuffleSpeed: 'shuffleMedium',
      shuffleTypes: ['Slow', 'Medium', 'Fast'],
      isDeckShuffled: false,
      shuffleCount: 0
    };
  },
  methods: {
    displayInitialDeck () {
      let id = 1;
      this.cards = [];
      let card = {};
      this.suits.forEach(suit => {
        this.ranks.forEach(rank => {
          card = {
            id: id,
            rank: rank,
            suit: suit
          };
          this.cards.push(card);
        });
        id++;
      });
    },
    shuffleDeck () {
      let i = this.cards.length;
      let randomNumber;
      let temp;
      while (--i > 0) {
        randomNumber = Math.floor(Math.random() * i);
        temp = this.cards[i];
        Vue.set(this.cards, i, this.cards[randomNumber]);
        Vue.set(this.cards, randomNumber, temp);
      }
    }
  },
  created () {
    this.displayInitialDeck();
  }
};
</script>

<style lang="less">
html, body, #app {
  height: 100%;
  background: ghostwhite;
}

.title {
  font-family: Roboto Slab, sans-serif;
  text-align: center;
  padding-top: 30px;
  margin-bottom: 0 !important;
  font-weight: 300;
  font-size: 3rem;
}

.vue-logo {
  height: 55px;
  position: relative;
  top: 10px;
}

.speed-buttons {
  text-align: center;
  padding-top: 30px;
}
.speed-buttons .button {
  height: 2.50em;
}

.main-buttons {
  display: block;
  margin: 0 auto;
  text-align: center;
  padding-top: 30px;
  margin-bottom: 0 !important;
}

.count-section {
  position: absolute;
  top: 10px;
  right: 10px;
}

.fas {
  padding-left: 5px;
}

.deck {
  margin-left: 30px;
  padding-top: 30px;
}

.card {
  width: 75px;
  height: 100px;
  float: left;
  margin-right: 5px;
  margin-bottom: 5px;
  border-radius: 2px;
}

.card__suit {
  width: 100%;
  display: block;
}

.card__suit--top {
  text-align: left;
  padding-left: 5px;
}

.card__suit--bottom {
  position: absolute;
  bottom: 0px;
  text-align: left;
  transform: rotate(180deg);
  padding-left: 5px;
}

.card__number {
  width: 100%;
  position: absolute;
  top: 38%;
  text-align: center;
}

.red {
  color: #FF0000;
}

.black {
  color: #000;
}

.twitter-section {
  position: absolute;
  bottom: 10px;
  right: 10px;
}
.twitter-section .fa-twitter-square {
  color: #00d1b2;
  font-size: 30px;
}

.shuffleSlow-move {
  transition: transform 2s;
}

.shuffleMedium-move {
  transition: transform 1s;
}

.shuffleFast-move {
  transition: transform 0.5s;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

@media (max-width: 1210px) {
  .deck {
    position: initial;
    top: 0;
  }

  .twitter-section {
    display: none;
  }
}
</style>
