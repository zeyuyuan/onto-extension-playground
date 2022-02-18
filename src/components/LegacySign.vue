<template>
  <n-list-item>
    <n-thing title="test sign">
      <n-space vertical>
        <n-input
          v-model:value="messageToSign"
          type="text"
          placeholder="Please input message to sign"
        />
        <n-button @click="signWithWeb3"> sign with web3 </n-button>
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

// TBD
const signWithWeb3 = async () => {
  const web3 = new Web3(injectedProvider.value);
  console.log(injectedProvider.value);
  const result = await web3.eth.sign(
    // messageToSign.value,
    web3.utils.utf8ToHex(messageToSign.value),
    injectedProvider.value.selectedAddress
  );
  web3Result.value = result;
  console.log("signWithWeb3", result);
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
