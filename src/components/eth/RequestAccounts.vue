<template>
  <n-list-item id="eth_requestAccounts">
    <n-thing title="eth_requestAccounts">
      <n-button @click="nativeConnect">Connect Wallet</n-button>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { NListItem, NThing, NButton } from "naive-ui";
import { inject } from "vue";

const provider = inject("provider");
const nativeConnect = async () => {
  const result = await provider.value.request({
    method: "eth_requestAccounts",
  });
  console.log(result);
};
</script>
