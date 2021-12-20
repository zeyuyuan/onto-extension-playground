<template>
  <n-list-item>
    <n-thing title="eth_blockNumber">
      <n-button @click="getChainId">eth_blockNumber</n-button>
      <span>{{ result }}</span>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { NListItem, NThing, NButton } from "naive-ui";
import { inject, ref } from "vue";

const provider = inject("provider");
const result = ref("");

const getChainId = async () => {
  result.value = await provider.value.request({
    method: "eth_blockNumber",
    params: [],
  });
  console.log(result.value);
};
</script>
