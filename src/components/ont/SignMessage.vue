<template>
  <n-list-item>
    <n-thing title="message.signMessage">
      <n-space vertical>
        <n-input v-model:value="text" placeholder="Message to sign" />
        <n-button @click="signMessage" :disabled="text.length === 0">
          Sign message
        </n-button>
        <n-button @click="signMessageUseIdentity" :disabled="text.length === 0">
          Sign message use identity
        </n-button>
      </n-space>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { inject, ref } from "vue";
import { NListItem, NThing, NButton, NSpace, NInput } from "naive-ui";

const client = inject("connectedClient");
const text = ref("");

const signMessage = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.message.signMessage({
    message: text.value,
    useIdentity: false,
  });
  console.log("result", result);
};

const signMessageUseIdentity = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.message.signMessage({
    message: text.value,
    useIdentity: true,
  });
  console.log("result", result);
};
</script>

<style lang="scss" scoped></style>
