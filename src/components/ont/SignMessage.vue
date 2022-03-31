<template>
  <n-list-item>
    <n-thing title="message.signMessage">
      <n-button @click="signMessage">Sign message</n-button>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { inject } from "vue";
import { NListItem, NThing, NButton } from "naive-ui";

const client = inject("connectedClient");

const signMessage = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.message.signMessage({
    message: "hello",
    useIdentity: false,
  });
  console.log("result", result);
};
</script>

<style lang="scss" scoped></style>
