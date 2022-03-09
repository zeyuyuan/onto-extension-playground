<template>
  <n-list-item>
    <n-thing title="GetEncryptionPublicKey">
      <n-space align="center">
        <n-button @click="getEncryptionPublicKey">
          getEncryptionPublicKey
        </n-button>
        <span>{{ result }}</span>
      </n-space>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { NListItem, NThing, NButton, NSpace } from "naive-ui";
import { ref, inject } from "vue";

const injectedProvider = inject("provider");
const result = ref("");

const getEncryptionPublicKey = async () => {
  if (!injectedProvider.value) {
    return;
  }
  const key = await injectedProvider.value.request({
    method: "eth_getEncryptionPublicKey",
    params: [injectedProvider.value.selectedAddress],
  });
  console.log("key", key);
  result.value = key;
};
</script>
