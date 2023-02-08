<template>
  Final price: €{{ price.toLocaleString() }} <br />
  trade orders: {{ tradeOrders }} | buy orders: {{ totalBought }} | sell orders:
  {{ totalSold }} <br />total spent buying: €{{ buyCash.toLocaleString() }}
  <br />
  total earned selling: €{{ sellCash.toLocaleString() }}
</template>
<script setup lang="ts">
import { ref, computed } from "vue";
const props = defineProps({
  sellOrders: { type: Number, required: true },
  buyOrders: { type: Number, required: true },
  tradeOrders: { type: Number, required: true },
});
const tradeAmount = computed(() => {
  return Math.abs(props.tradeOrders);
});
const basePrice = ref(300);
const totalSold = computed(() => {
  if (props.tradeOrders > 0) {
    return props.sellOrders + tradeAmount.value;
  }
  return props.sellOrders;
});
const totalBought = computed(() => {
  if (props.tradeOrders < 0) {
    return props.buyOrders + tradeAmount.value;
  }
  return props.buyOrders;
});
const price = computed(() => {
  if (props.tradeOrders > 0) {
    const orderRatio = props.buyOrders / (props.sellOrders + tradeAmount.value);
    return basePrice.value * orderRatio;
  }

  const orderRatio = (props.buyOrders + tradeAmount.value) / props.sellOrders;
  return basePrice.value * orderRatio;
});
const buyCash = computed(() => {
  return Math.floor(props.buyOrders * price.value);
});
const sellCash = computed(() => {
  return Math.floor(props.sellOrders * price.value);
});
</script>
