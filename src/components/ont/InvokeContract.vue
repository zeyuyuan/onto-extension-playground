<template>
  <n-list-item>
    <n-thing title="smartContract">
      <n-space vertical>
        <n-input v-model:value="contract" placeholder="Contract" />
        <n-input v-model:value="operate" placeholder="Operate" />
        <n-input v-model:value="params" placeholder="Params" />
        <n-input v-model:value="gasPrice" placeholder="Gas price(optional)" />
        <n-input v-model:value="gasLimit" placeholder="Gas limit(optional)" />
        <n-button @click="invoke" :disabled="!(contract && operate)">
          Invoke
        </n-button>
        <n-button @click="invokeWasm" :disabled="!(contract && operate)">
          Invoke Wasm
        </n-button>
      </n-space>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { inject, ref } from "vue";
import { NListItem, NThing, NButton, NSpace, NInput } from "naive-ui";

const defaultParams = [
  {
    type: "Address",
    value: "AV88PcsdFk2MTcPkuyPNEkpgLFiKHtCM1r",
  },
  {
    type: "Address",
    value: "AbvLRCr2uzqG5LJPsdw7zYKjuHFhPPX9VM",
  },
  {
    type: "Long",
    value: "1000000000",
  },
];

const client = inject("connectedClient");
const contract = ref("ff31ec74d01f7b7d45ed2add930f5d2239f7de33");
const operate = ref("transfer");
const params = ref(JSON.stringify(defaultParams));
const gasLimit = ref("");
const gasPrice = ref("");

const getGas = () => {
  return {
    gasPrice: Number(gasPrice.value) || undefined,
    gasLimit: Number(gasLimit.value) || undefined,
  };
};

const invoke = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.smartContract.invoke({
    scriptHash: contract.value,
    operation: operate.value,
    args: params.value ? JSON.parse(params.value) : [],
    ...getGas(),
  });
  console.log("invoke result:", result);
};

const invokeWasm = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.smartContract.invokeWasm({
    scriptHash: contract.value,
    operation: operate.value,
    args: params.value ? JSON.parse(params.value) : [],
    ...getGas(),
  });
  console.log("invoke result:", result);
};
</script>

<style lang="scss" scoped></style>
