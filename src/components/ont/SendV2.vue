<template>
  <n-list-item>
    <n-thing title="asset.sendV2">
      <n-button @click="sendOnt">Send ONT</n-button>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { inject } from "vue";
import { NListItem, NThing, NButton } from "naive-ui";

const client = inject("connectedClient");

const sendOnt = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.asset.sendV2({
    to: "AavQvmUXJBebmpahnXbXMu1cs1JuFtCYkd",
    asset: "ONG",
    amount: "2",
  });
  console.log("result", result);
};
</script>

<style lang="scss" scoped></style>
