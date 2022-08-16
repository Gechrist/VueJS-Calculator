<template>
  <button @click="$emit('computeInput', symbol)">{{ symbol }}</button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
export default defineComponent({
  props: {
    symbol: String,
  },
});
</script>
