<template>
  <li  
    class="memory-card"
    :class="{ flip: card.visible, disabled: card.matched }"
    :data-framework="card.name"
    @click="showCard(card, index)"
  >
    <img class="front-face" :src="card.image" :alt="card.name" />
    <img class="back-face" src="../assets/img/js-badge.svg" alt="JS Badge" />
  </li>
</template>

<script>
export default {
  props: {
    card: {
      type: Object,
      required: true,
    },
    index: {
        type: Number,
        required: true,
    }
  },
  methods: {
      showCard(card, index){
          this.$emit('showCard', card, index)
      }
  }
};
</script>

<style>
li {
    list-style: none;
}
</style>