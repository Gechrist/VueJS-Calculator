<template>
  <button @click="$emit('computeInput', symbol)">{{ symbol }}</button>
  <!-- <button @click="computeInput(symbol)">{{ symbol }}</button> -->
</template>

<script lang="ts">
export default {
  props: {
    symbol: String,
    // computeInput: Function,
  },
};
</script>
