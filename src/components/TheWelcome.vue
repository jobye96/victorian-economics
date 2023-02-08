<template>
  <WelcomeItem>
    <template #icon>
      <DocumentationIcon />
    </template>
    <template #heading>1. Resource Extraction</template>

    The first step in the economic process is using Labour and possibly Tools to
    extract a Resource.
    <ResourceExtraction
      resource-type="grain"
      @sell-orders="(n: number) => (totalOrder = n)"
    />
  </WelcomeItem>
  <WelcomeItem>
    <template #icon>
      <InfrastructureIcon />
    </template>
    <template #heading>2. Market Access</template>

    The amount of resources on the market depends on Infrastructure.
    <MarketAccess @market-access="(n: number) => (access = n)" />
  </WelcomeItem>
  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>3. Industry</template>

    Industries turn Resources into other goods, using Labour and Tools.
    Infrastructure also affects how much of this goes to the market.
    <GoodsIndustry @buy-orders="(n:number) => (industryBought = n)" />
  </WelcomeItem>
  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>4. Consumption</template>

    The population consumes goods depending on their Wealth. This creates buy
    orders on the market. <br />
    <PopConsumption @buy-orders="(n:number) => (popBought = n)" />
  </WelcomeItem>
  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>5. Inter-market Trade</template>

    A preliminary market price is set from the amount of buy and sell orders.
    This price affects how much of the goods that are traded. The profit goes to
    traders in trade centers.<br />
    <PreMarketPrice :buy-orders="totalBought" :sell-orders="accessOrder" />
  </WelcomeItem>
  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>6. Market prices</template>
    The total amount of buy- and sell orders from buildings, pops and trade go
    together to calculate the nationwide market price of goods. Every agent that
    has bought or sold goods receives (or pays) an amount of £ to/from its
    wallet.
  </WelcomeItem>
  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>7. Wages</template>
    Every worker now recieves a wage from its building, depending on their
    profession.
  </WelcomeItem>
  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>8. Taxes</template>
    The population pays taxes based on their income and which goods they have
    purchased. The money from tax goes to the state treasury, which can be spent
    on constructing new buildings or subsidizing current ones.
  </WelcomeItem>
  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>9. Budget readjustment</template>
    Every part of the population now adjusts their Wealth up or down according
    to current incomes and expenditures. This affects what purchases they make
    next time.
  </WelcomeItem>
  <WelcomeItem>
    <template #icon>
      <SupportIcon />
    </template>
    <template #heading>10. Reset</template>
    All economic data is now wiped clean, except for what has been stored as £
    in buildings or treasuries or Wealth in population.
  </WelcomeItem>
</template>

<script setup lang="ts">
import { ref, computed } from "vue";
import WelcomeItem from "./WelcomeItem.vue";
import DocumentationIcon from "./icons/IconDocumentation.vue";
import InfrastructureIcon from "./icons/IconInfrastructure.vue";
import SupportIcon from "./icons/IconSupport.vue";
import ResourceExtraction from "@/components/ResourceExtraction.vue";
import MarketAccess from "@/components/MarketAccess.vue";
import GoodsIndustry from "@/components/GoodsIndustry.vue";
import PopConsumption from "@/components/PopConsumption.vue";
import PreMarketPrice from "@/components/PreMarketPrice.vue";
// import PostMarketPrice from "@components/PostMarketPrice.vue";
const totalOrder = ref(4000);
const accessOrder = computed(() => {
  return (totalOrder.value * access.value) / 100;
});
const industryBought = ref(3000);
const popBought = ref(12000);
const access = ref(100);
const totalBought = computed(() => {
  return ((popBought.value + industryBought.value) * access.value) / 100;
});
</script>
