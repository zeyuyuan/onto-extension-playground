<template>
  <n-list-item>
    <n-thing title="asset.approve">
      <n-button @click="approveOnt">approve ONT</n-button>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { inject } from "vue";
import { NListItem, NThing, NButton } from "naive-ui";
const client = inject("connectedClient");
const account = inject("account");

const approveOnt = async () => {
  if (!client.value || !account.value) {
    throw new Error("connect first");
  }
  const result = await client.value.asset.approve({
    from: account.value,
    to: "AavQvmUXJBebmpahnXbXMu1cs1JuFtCYkd",
    asset: "ONG",
    amount: "2",
  });
  console.log("result", result);
};
</script>

<style lang="scss" scoped></style>
