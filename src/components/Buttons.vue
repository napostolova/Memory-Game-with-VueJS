<template>
  <div class="buttons">
    <button class="btn" @click="onStartGame" :disabled="isGameStarted">
      Start new game
    </button>
    <button class="btn" @click="onResetGame" :disabled="!isGameStarted">
      Reset game
    </button>
     <div class="counter">
        <h1>{{ counter }} s.</h1>
      </div>
  </div>
</template>

<script>
export default {
  props: {
    isGameStarted: {
      type: Boolean,
      required: true,
    },
    counter: {
        type: Number,
        required: true
    }
  },
  methods: {
    onStartGame() {
      this.$emit("onStartGame");
    },
    onResetGame() {
      this.$emit("onResetGame");
    },
  },
};
</script>

<style>
</style>