<template>
  Grain has a base price of €{{ basePrice }} a unit. The price goes up or down
  depending on buy and sell orders. <br />
  There are {{ buyOrders.toLocaleString() }} buy orders and
  {{ sellOrders.toLocaleString() }} sell orders for grain. This results in a
  market price of €{{ marketPrice.toLocaleString() }}.
  <br />
  The neighboring country of Vuezuela produces {{ NeighborText }}. This results
  in a price of €{{ tradePrice }} in their market. <br />
  Traders working in our country try to profit off of the price difference
  between the two countries' markets. Thus, they they buy or sell a number of
  goods depending on the order difference in our market and the trade price of
  the other market.
  <br />
  Our traders buy/ sell
  {{ tradeAmount.toLocaleString() }}
  goods. They pay €{{ tradeCost.toLocaleString() }} to buy from one market and
  sell for €{{ tradeRevenue.toLocaleString() }} in the other. They earn €{{
    perGoodsProfit.toLocaleString()
  }}
  per item traded.
  <br />
  <div>
    <input
      v-model="tradePrice"
      id="trade-price"
      type="range"
      min="100"
      max="1000"
      step="10"
    />
    <label for="trade-price">Grain price in Vuezuela: €{{ tradePrice }}</label>
  </div>
</template>
<script setup lang="ts">
import { computed, ref } from "vue";
const props = defineProps({
  sellOrders: { type: Number, required: true },
  buyOrders: { type: Number, required: true },
});
const NeighborText = computed(() => {
  if (tradePrice.value < 280) {
    return "a lot of grain for its small population";
  } else if (tradePrice.value < 350) {
    return "an adequate amount of grain for its population";
  } else return "little grain for its population";
});
const basePrice = ref(300);
const tradePrice = ref(250);

const marketPrice = computed(() => {
  const orderRatio = props.buyOrders / props.sellOrders;
  return basePrice.value * orderRatio;
});
const orderDifference = computed(() => {
  return Math.abs(props.sellOrders - props.buyOrders);
});
// TODO: the trade amount should be dependent on the trade price somehow
const tradeAmount = computed(() => {
  return 0.5 * orderDifference.value;
});

const tradeCost = computed(() => {
  if (marketPrice.value > tradePrice.value) {
    return Math.floor(tradePrice.value * tradeAmount.value);
  } else {
    return Math.floor(marketPrice.value * tradeAmount.value);
  }
});
const tradeRevenue = computed(() => {
  if (marketPrice.value > tradePrice.value) {
    return Math.floor(marketPrice.value * tradeAmount.value);
  } else {
    return Math.floor(tradePrice.value * tradeAmount.value);
  }
});
const perGoodsProfit = computed(() => {
  return (tradeRevenue.value - tradeCost.value) / tradeAmount.value;
});
</script>
