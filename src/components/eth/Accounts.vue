<template>
  <n-list-item>
    <n-thing title="eth_accounts">
      <n-button @click="getAccounts">Get accounts</n-button>
      <span>{{ result }}</span>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { NListItem, NThing, NButton } from "naive-ui";
import { inject, ref } from "vue";

const provider = inject("provider");
const result = ref("");

const getAccounts = async () => {
  result.value = await provider.value.request({
    method: "eth_accounts",
  });
};
</script>
