<template>
  <BaseLayout>
    <template v-slot:menu>
      <n-anchor
        listen-to=".layout__right"
        :bound="24"
        :show-rail="false"
        type="block"
      >
        <n-anchor-link title="connect" href="#ont_connect" />
        <n-anchor-link title="asset.send" href="#asset_send" />
        <n-anchor-link title="asset.sendV2" href="#asset_send_v2" />
        <n-anchor-link title="Governance" href="#governance" />
        <n-anchor-link title="message.signMessage" href="#sign_message" />
        <n-anchor-link title="smartContract" href="#invoke_contract" />
      </n-anchor>
    </template>
    <n-list bordered>
      <n-list-item>
        <n-thing id="ont_connect" title="test connect">
          <n-space v-if="account">
            <span>{{ isCyano ? "Cyano" : "ONTO" }}</span>
            <span>{{ account }}</span>
          </n-space>
          <n-space v-else>
            <n-button @click="connectOnto">Connect ONTO</n-button>
            <n-button @click="connectCyano">Connect Cyano</n-button>
          </n-space>
        </n-thing>
      </n-list-item>
      <LegacySend id="asset_send" />
      <SendV2 id="asset_send_v2" />
      <SendGovernance id="governance" />
      <SignMessage id="sign_message" />
      <InvokeContract id="invoke_contract" />
    </n-list>
  </BaseLayout>
</template>

<script setup>
import {
  NList,
  NListItem,
  NThing,
  NButton,
  NSpace,
  NAnchor,
  NAnchorLink,
} from "naive-ui";
import { client, ExtensionType } from "@ont-dev/ontology-dapi";
import { provide, ref } from "vue";
import BaseLayout from "../components/eth/BaseLayout";
import LegacySend from "../components/ont/LegacySend";
import SendV2 from "../components/ont/SendV2";
import SendGovernance from "../components/ont/SendGovernance";
import SignMessage from "../components/ont/SignMessage";
import InvokeContract from "../components/ont/InvokeContract";

const connectedClient = ref(null);
const account = ref("");
const isCyano = ref(false);
provide("connectedClient", connectedClient);
provide("account", account);

const connectOnto = async () => {
  connectedClient.value = client.registerClient({
    logMessages: true,
    logWarnings: true,
    extension: ExtensionType.Onto,
  });
  account.value = await connectedClient.value.asset.getAccount();
  isCyano.value = false;
};

const connectCyano = async () => {
  connectedClient.value = client.registerClient({
    logMessages: true,
    logWarnings: true,
    extension: ExtensionType.Cyano,
  });
  account.value = await connectedClient.value.asset.getAccount();
  isCyano.value = true;
};
</script>
