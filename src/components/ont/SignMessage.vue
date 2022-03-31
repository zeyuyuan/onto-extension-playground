<template>
  <n-list-item>
    <n-thing title="message.signMessage">
      <n-space vertical>
        <n-input v-model:value="text" placeholder="Message to sign/verify" />
        <n-switch v-model:value="isUseIdentity">Use identity</n-switch>
        <n-button @click="signMessage" :disabled="text.length === 0">
          Sign message
        </n-button>
        <n-input v-model:value="signature" placeholder="Signature to verify" />
        <n-input v-model:value="publicKey" placeholder="Public key to verify" />
        <n-button @click="verifyMessage" :disabled="!(signature && publicKey)">
          Verify message
        </n-button>
      </n-space>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { inject, ref } from "vue";
import { NListItem, NThing, NButton, NSpace, NInput, NSwitch } from "naive-ui";

const client = inject("connectedClient");
const text = ref("");
const publicKey = ref("");
const signature = ref("");
const isUseIdentity = ref(false);

const signMessage = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.message.signMessage({
    message: text.value,
    useIdentity: isUseIdentity.value,
  });
  publicKey.value = result.publicKey;
  signature.value = result.data;
  console.log("signMessage result:", result);
};

const verifyMessage = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.message.verifyMessage({
    message: text.value,
    signature: {
      publicKey: publicKey.value,
      data: signature.value,
    },
  });
  console.log("verifyMessage result:", result);
};
</script>

<style lang="scss" scoped></style>
