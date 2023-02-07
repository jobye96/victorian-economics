<template>
  <p>
    This {{ resourceType }} {{ building() }} produces {{ resourceLoad }}
    {{ resourceType }} units employing {{ numWorkers.toLocaleString() }} people.
  </p>
  <label for="is-tooled">Use Tools? </label>
  <input v-model="isTooled" type="checkbox" id="is-tooled" /><br />
  <div>
    <input
      v-model="numWorkers"
      id="num-workers"
      type="range"
      max="5000"
      step="500"
    />
    <label for="num-workers">Number of workers: {{ numWorkers }}</label>
  </div>
  <br />
</template>
<script setup lang="ts">
import { computed, ref, watch } from "vue";
const props = defineProps({
  resourceType: String,
});
const resourceTypes = {
  grains: ["wheat", "grain", "rice"],
  minerals: ["iron", "coal", "lead", "sulfur"],
  woods: ["lumber"],
};
const building = (): String => {
  if (props.resourceType) {
    if (resourceTypes.grains.includes(props.resourceType)) {
      return "farm";
    } else if (resourceTypes.minerals.includes(props.resourceType)) {
      return "mine";
    } else {
      return "camp";
    }
  } else {
    return "building";
  }
};
const maxLabour = 5000;
const maxGrain = 10000;
const numWorkers = ref(2000);
const isTooled = ref(false);

const resourceLoad = computed(() => {
  const output = (numWorkers.value / maxLabour) * maxGrain;
  if (isTooled.value) {
    return output * 1.2;
  }
  return output;
});
const emit = defineEmits(["sell-orders"]);
watch(resourceLoad, (newLoad) => {
  emit("sell-orders", newLoad);
});
</script>
