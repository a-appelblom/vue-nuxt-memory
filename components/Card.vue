<template>
  <div>
    <img v-if="selected || locked" :src="card.image" alt="" />
    <img v-if="!selected && !locked" :src="joker" alt="" />
  </div>
</template>

<script>
export default {
  props: {
    card: {
      type: Object,
      default: () => {},
    },
    selected: {
      type: Boolean,
      default: false,
    },
    matched: {
      type: Boolean,
      default: false,
    },
    resetCards: {
      type: Function,
      default: () => {},
    },
  },
  data() {
    return {
      locked: false,
      joker: 'https://deckofcardsapi.com/static/img/X1.png',
    }
  },
  watch: {
    matched(curr) {
      if (curr && this.selected) {
        this.locked = true
        this.resetCards()
      }
    },
  },
}
</script>
