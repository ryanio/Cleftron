<script>
import ApproveTx from './ApproveTx.vue';
import ApproveExport from './ApproveExport.vue';
import ApproveImport from './ApproveImport.vue';
import ApproveListing from './ApproveListing.vue';
import ApproveSignData from './ApproveSignData.vue';
import ApproveNewAccount from './ApproveNewAccount';
import Logger from './Logger.vue';
import TaskQueue from './TaskQueue.vue';
import { ipcRenderer } from 'electron';
import Vue from 'vue';
import store from '@/store';
export default {
  components: {
    TaskQueue,
    Logger,
    ApproveTx,
    ApproveExport,
    ApproveImport,
    ApproveListing,
    ApproveSignData,
    ApproveNewAccount
  },
  data() {
    return {
      store: store,
      mounted: null
    };
  },
  created: function() {
    ipcRenderer.on('ApprovalRequired', (e, m) => {
      store.dispatch('addData', m);
    });
    // Let the main process know the page is loaded
    ipcRenderer.send('channelsConfigured', 'ping');
  }
};
</script>


<template>
  <div>
    <taskQueue></taskQueue>
    <b-container fluid>
        <component :is="store.state.ui"></component>
    </b-container>
    <logger></logger>
  </div>
</template>
