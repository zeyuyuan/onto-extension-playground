<template>
  <n-list-item>
    <n-thing title="estimateGas of erc20 approve">
      <n-space align="center">
        <n-button @click="estimateGasWithEthers">
          estimateGas with ethers
        </n-button>
        <span>{{ ethersResult }}</span>
        <n-button @click="estimateGasWithWeb3">
          estimateGas with web3
        </n-button>
        <span>{{ web3Result }}</span>
      </n-space>
    </n-thing>
  </n-list-item>
</template>

<script setup>
import { NListItem, NThing, NButton, NSpace } from "naive-ui";
import { ref, inject } from "vue";
import { ethers } from "ethers";
import Web3 from "web3";

const injectedProvider = inject("provider");
const ethersResult = ref("");
const web3Result = ref("");

const ERC20_ABI =
  '[{"inputs":[{"internalType":"address","name":"lockProxyContractAddress","type":"address"}],"payable":false,"stateMutability":"nonpayable","type":"constructor"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"owner","type":"address"},{"indexed":true,"internalType":"address","name":"spender","type":"address"},{"indexed":false,"internalType":"uint256","name":"value","type":"uint256"}],"name":"Approval","type":"event"},{"anonymous":false,"inputs":[{"indexed":true,"internalType":"address","name":"from","type":"address"},{"indexed":true,"internalType":"address","name":"to","type":"address"},{"indexed":false,"internalType":"uint256","name":"value","type":"uint256"}],"name":"Transfer","type":"event"},{"constant":true,"inputs":[{"internalType":"address","name":"owner","type":"address"},{"internalType":"address","name":"spender","type":"address"}],"name":"allowance","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"spender","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"approve","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[{"internalType":"address","name":"account","type":"address"}],"name":"balanceOf","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"decimals","outputs":[{"internalType":"uint8","name":"","type":"uint8"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"spender","type":"address"},{"internalType":"uint256","name":"subtractedValue","type":"uint256"}],"name":"decreaseAllowance","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"spender","type":"address"},{"internalType":"uint256","name":"addedValue","type":"uint256"}],"name":"increaseAllowance","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"name","outputs":[{"internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"symbol","outputs":[{"internalType":"string","name":"","type":"string"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"recipient","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"transfer","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":false,"inputs":[{"internalType":"address","name":"sender","type":"address"},{"internalType":"address","name":"recipient","type":"address"},{"internalType":"uint256","name":"amount","type":"uint256"}],"name":"transferFrom","outputs":[{"internalType":"bool","name":"","type":"bool"}],"payable":false,"stateMutability":"nonpayable","type":"function"}]';
const ERC20_CONTRACT = "0xA3F0Ed8B36258849E1dfeAfCDFE34E8b64D95D84";

const estimateGasWithEthers = async () => {
  let provider = new ethers.providers.Web3Provider(injectedProvider.value);
  let contract = new ethers.Contract(
    ERC20_CONTRACT,
    JSON.parse(ERC20_ABI),
    provider
  );
  const gas = await contract.estimateGas.approve(
    "0xA3F0Ed8B36258849E1dfeAfCDFE34E8b64D95D84",
    "100000000"
  );
  ethersResult.value = gas.toString();
  console.log("estimateGasWithEthers", gas);
};

const estimateGasWithWeb3 = async () => {
  const web3 = new Web3(injectedProvider.value);
  const contract = new web3.eth.Contract(JSON.parse(ERC20_ABI), ERC20_CONTRACT);
  const [account] = await web3.eth.getAccounts();
  const instance = await contract.methods.approve(account, "100000000");
  const result = await instance.estimateGas({
    from: account,
  });
  web3Result.value = result;
  console.log("estimateGasWithWeb3", result);
};
</script>
