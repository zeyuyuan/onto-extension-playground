<template>
  <n-list-item>
    <n-thing
      title="eth_chainId"
      title-extra="(For onto wallet,you need to be connected before get chain id)"
    >
      <n-button @click="getChainId">Get chain id</n-button>
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
    method: "eth_chainId",
  });
};
</script>
