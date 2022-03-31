<template>
  <n-list-item>
    <n-thing title="governance.stakePeers(open chrome console)">
      <n-space vertical>
        <n-select
          v-model:value="selectedPeers"
          multiple
          :options="peers"
          placeholder="Select peers(Get all peers first)"
        />
        <n-input
          v-model:value="amountInput"
          placeholder="Input amounts(1,2,3)"
        />
        <div>{{ amountInput }}</div>
        <n-input v-model:value="gasPrice" placeholder="Gas price(optional)" />
        <n-input v-model:value="gasLimit" placeholder="Gas limit(optional)" />
        <n-space>
          <n-button @click="getAll">Get all peers</n-button>
        </n-space>
        <n-space>
          <n-button @click="stakePeers">Stake peers</n-button>
          <n-button @click="redeemPeers">Redeem peers</n-button>
          <n-button @click="withdrawReward">Withdraw fee reward</n-button>
          <n-button @click="withdrawUnfrozenOnt"
            >Withdraw unfrozen ont</n-button
          >
          <n-button @click="withdrawUnbound"
            >Withdraw peers unbound ong</n-button
          >
        </n-space>
        <n-space>
          <n-button @click="getMyStake">Get my stake of first peer</n-button>
          <n-button @click="getAttributes"
            >Get attributes of first peer</n-button
          >
          <n-button @click="getReward">Get reward</n-button>
        </n-space>
      </n-space>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { inject, ref } from "vue";
import { NListItem, NThing, NButton, NSpace, NSelect, NInput } from "naive-ui";

const client = inject("connectedClient");
const peers = ref([]);
const selectedPeers = ref([
  "02ae45c8813a51906a76150e69d274a9972665f61edc45b30ba7043376c4c4c64d",
  "02bd20195047b02c52e4edf4f49545fc9125affc25e845b0fdf936318ef9df06ac",
]);
const amountInput = ref("1,2");
const gasLimit = ref("");
const gasPrice = ref("");

const getGas = () => {
  return {
    gasPrice: Number(gasPrice.value) || undefined,
    gasLimit: Number(gasLimit.value) || undefined,
  };
};

const stakePeers = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.governance.stakePeers({
    peerPubKeys: [...selectedPeers.value],
    amounts: amountInput.value.split(","),
    ...getGas(),
  });
  console.log("stakePeers result:", result);
};

const redeemPeers = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.governance.redeemPeers({
    peerPubKeys: [...selectedPeers.value],
    amounts: amountInput.value.split(","),
    ...getGas(),
  });
  console.log("redeemPeers result:", result);
};

const withdrawReward = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.governance.withdrawFeeReward({
    ...getGas(),
  });
  console.log("withdrawReward result:", result);
};

const withdrawUnfrozenOnt = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.governance.withdrawUnfrozenOnt({
    peerPubKeys: [...selectedPeers.value],
    amounts: amountInput.value.split(","),
    ...getGas(),
  });
  console.log("withdrawUnfrozenOnt result:", result);
};

const withdrawUnbound = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.governance.withdrawPeerUnboundOng({
    ...getGas(),
  });
  console.log("withdrawPeerUnboundOng result:", result);
};

const getAll = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.governance.getAllPeerPool();
  console.log("getAllPeers result:", result);
  peers.value = result.map((item) => ({
    label: item.peerPubKey,
    value: item.peerPubKey,
  }));
};

const getMyStake = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.governance.getStakeInfo({
    peerPubKey: selectedPeers.value[0],
  });
  console.log("Get my stake of first result:", result);
};

const getAttributes = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.governance.getAttributes({
    peerPubKey: selectedPeers.value[0],
  });
  console.log("getAttributes result:", result);
};

const getReward = async () => {
  if (!client.value) {
    throw new Error("connect first");
  }
  const result = await client.value.governance.getRewardFeeAmount();
  console.log("getReward result:", result);
};
</script>

<style lang="scss" scoped></style>
