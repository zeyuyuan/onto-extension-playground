<template>
  <n-list-item>
    <n-thing title="get gas price">
      <n-space align="center">
        <n-button @click="getGasPriceWithEthers" disabled>
          get gas price with ethers
        </n-button>
        <span>{{ ethersResult }}</span>
        <n-button @click="getGasPriceWithWeb3">
          get gas price with web3
        </n-button>
        <span>{{ web3Result }}</span>
      </n-space>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { NListItem, NThing, NButton, NSpace } from "naive-ui";
import { ref, inject } from "vue";
import Web3 from "web3";

const injectedProvider = inject("provider");
const ethersResult = ref("");
const web3Result = ref("");

const getGasPriceWithEthers = async () => {
  console.log("TBD");
};

const getGasPriceWithWeb3 = async () => {
  const web3 = new Web3(injectedProvider.value);
  const result = await web3.eth.getBlock("latest");
  web3Result.value = result;
  console.log("getGasPriceWithWeb3", result);
};
</script>
