<template>
  takes in trade orders and all other market orders returns € cash that goes to
  wallets of ordering agents
</template>
<script setup lang="ts">
import { ref, computed } from "vue";
const props = defineProps({
  sellOrders: { type: Number, required: true },
  buyOrders: { type: Number, required: true },
});
</script>
