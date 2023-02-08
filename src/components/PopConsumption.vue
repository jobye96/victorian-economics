<template>
  There are {{ totalPop.toLocaleString() }} people living in this state. <br />
  The average demand per person is {{ defaultDemand }} units of grain. <br />
  {{ childPop.toLocaleString() }} of the population are children, consuming 0.03
  units of grain because of their reduced Wealth and needs.<br />
  {{ upperclassPop.toLocaleString() }} people are upper class, consuming 0.2
  units per person. <br />
  The total buy orders for grain in the province is ({{ defaultOrders }} +
  {{ childOrders }} + {{ upperclassOrders }}) = <br />
  {{ buyOrders.toLocaleString() }}
  <input
    v-model="totalPop"
    id="state-pop"
    type="range"
    max="1000000"
    step="10000"
  />
  <label for="state-pop">Population in the state: {{ totalPop }}</label>
</template>
<script setup lang="ts">
import { ref, computed, watch } from "vue";
const totalPop = ref(500000);
const childPercent = 0.5;
const childPop = computed(() => {
  return totalPop.value * childPercent;
});
const upperclassPercent = 0.02;
const upperclassPop = computed(() => {
  return totalPop.value * upperclassPercent;
});
const defaultPop = computed(() => {
  return totalPop.value - childPop.value - upperclassPop.value;
});
const defaultDemand = 0.05;
const childDemand = 0.03;
const upperclassDemand = 0.2;
const defaultOrders = computed(() => {
  return defaultDemand * defaultPop.value;
});

const childOrders = computed(() => {
  return childDemand * childPop.value;
});

const upperclassOrders = computed(() => {
  return upperclassDemand * upperclassPop.value;
});
const buyOrders = computed(() => {
  return (
    defaultPop.value * defaultDemand +
    childPop.value * childDemand +
    upperclassPop.value * upperclassDemand
  );
});

const emit = defineEmits(["buy-orders"]);

watch(buyOrders, (newOrders) => {
  emit("buy-orders", newOrders);
});
</script>
