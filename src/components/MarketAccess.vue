<template>
  <p>
    The amount of goods going to the market is {{ access }} %, using
    {{ infrastructureUsage }} infrastructure out of
    {{ maxInfrastructure }} available.
  </p>
  <input
    v-model="infrastructureUsage"
    id="infra-usage"
    type="range"
    max="200"
  />
  <label for="infra-usage"
    >Infrastructure used: {{ infrastructureUsage }}</label
  >
</template>
<script setup lang="ts">
import { computed, ref, watch } from "vue";
const infrastructureUsage = ref(20);
const maxInfrastructure = 50;
const access = computed(() => {
  if (infrastructureUsage.value > maxInfrastructure) {
    return Math.floor((maxInfrastructure / infrastructureUsage.value) * 100);
  } else {
    return 100;
  }
});
const emit = defineEmits(["market-access"]);
watch(access, (newAccess) => {
  emit("market-access", newAccess);
});
</script>
