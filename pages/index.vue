<template>
  <div class="grid grid-cols-12 gap-4 p-8 min-h-screen max-w-7xl mx-auto">
    <div v-for="card of cards" :key="card.code">
      <div @click="setCards(card)">
        <Card
          :card="card"
          :selected="checkSelected(card)"
          :matched="matched"
          :reset-cards="resetCards"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      cards: [],
      first: { code: '' },
      second: { code: '' },
      matched: false,
      bothLocked: false,
    }
  },
  async fetch() {
    async function getData(url) {
      const res = await fetch(url)
      const data = await res.json()
      return data
    }
    const deck = await getData(
      'https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=1'
    )
    const cards = await getData(
      `https://deckofcardsapi.com/api/deck/${deck.deck_id}/draw/?count=52`
    )
    this.cards = cards.cards
  },
  methods: {
    setCards(card) {
      if (!this.first.code) {
        this.first = card
      } else if (!this.second.code && this.first.code !== card.code) {
        this.second = card
      } else if (
        this.first.value === this.second.value &&
        this.getColor(this.first) === this.getColor(this.second)
      ) {
        this.matched = true
      } else {
        this.first = { code: '' }
        this.second = { code: '' }
      }
    },

    getColor(card) {
      if (card.suit === 'DIAMONDS' || card.suit === 'HEARTS') return 'red'
      return 'black'
    },

    checkSelected(card) {
      if (this.first.code === card.code || this.second.code === card.code)
        return true
      return false
    },

    resetCards() {
      if (!this.bothLocked) {
        this.bothLocked = true
      } else {
        this.first = { code: '' }
        this.second = { code: '' }
        this.matched = false
        this.bothLocked = false
      }
    },
  },
}
</script>
