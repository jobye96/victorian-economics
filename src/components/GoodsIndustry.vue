<template>
  <p>
    This industry uses {{ grainInput }} grain and {{ toolInput }} tools. It
    produces {{ good }} groceries employing {{ labour }} people.
  </p>
  <input type="range" v-model="labour" max="5000" step="500" />
</template>
<script setup lang="ts">
import { ref, computed, watch } from "vue";
const maxGrain = 5000;
const maxTools = 2000;

const grainInput = computed(() => {
  return employment.value * maxGrain;
});

const toolInput = computed(() => {
  return employment.value * maxTools;
});

const labour = ref(3000);
const maxLabour = 5000;
const maxOutput = 10000;
const employment = computed(() => {
  return labour.value / maxLabour;
});

const good = computed(() => {
  return maxOutput * employment.value;
});

const emit = defineEmits(["buy-orders"]);
watch(grainInput, (newInput) => {
  emit("buy-orders", newInput);
});
</script>
