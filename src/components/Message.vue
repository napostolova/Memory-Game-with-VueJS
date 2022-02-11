<template>
  <div class="result" v-if="isGameOver">
    <p v-if="areAllFound" class="win">YOU ARE WINNER!</p>
    <p v-else class="fail">YOU LOST THE GAME!</p>
  </div>
</template>

<script>
export default {
    props: {
        isGameOver: Boolean,
        areAllFound: {
            type: Boolean,
            required: true
        }
    }
};
</script>

<style>
</style>