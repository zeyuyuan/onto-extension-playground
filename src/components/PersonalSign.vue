<template>
  <n-list-item>
    <n-thing title="test personal_sign">
      <n-space vertical>
        <n-input
          v-model:value="messageToSign"
          type="text"
          placeholder="Please input message to sign"
        />
        <n-button @click="personalSignWithWeb3">
          personal_sign with web3
        </n-button>
        <span>{{ web3Result }}</span>
        <n-button @click="ecRecoverWithWeb3"> ecRecover with web3 </n-button>
        <span>{{ web3RecoverResult }}</span>
      </n-space>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { NListItem, NThing, NButton, NSpace, NInput } from "naive-ui";
import { ref, inject } from "vue";
import Web3 from "web3";

const injectedProvider = inject("provider");
const web3Result = ref("");
const web3RecoverResult = ref("");
const messageToSign = ref("Hello world");

const personalSignWithWeb3 = async () => {
  const web3 = new Web3(injectedProvider.value);
  const result = await web3.eth.personal.sign(
    messageToSign.value,
    "0xa96F919b23D98695263E62591aede3a5467BF1fA",
    ""
  );
  web3Result.value = result;
  console.log("getGasPriceWithWeb3", result);
};

const ecRecoverWithWeb3 = async () => {
  const web3 = new Web3(injectedProvider.value);
  const result = await web3.eth.personal.ecRecover(
    messageToSign.value,
    web3Result.value
  );
  web3RecoverResult.value = result;
  console.log(ecRecoverWithWeb3, result);
};
</script>
