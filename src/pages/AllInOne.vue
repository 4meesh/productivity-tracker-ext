<template>
  <div class="all-in-one">
    <div class="tabs">
      <button v-for="tab in tabs" :key="tab" :class="{active: activeTab === tab}" @click="activeTab = tab">
        {{ tab }}
      </button>
    </div>
    <div class="tab-content">
      <div v-if="activeTab === 'Dashboard'">
        <DashboadContainer />
      </div>
      <div v-if="activeTab === 'Whitelist'">
        <WhiteList />
      </div>
      <div v-if="activeTab === 'Limits'">
        <Limits />
      </div>
      <div v-if="activeTab === 'Notifications'">
        <Notifications />
      </div>
      <div v-if="activeTab === 'General Settings'">
        <GeneralSettings />
      </div>
      <div v-if="activeTab === 'About'">
        <About />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import DashboadContainer from '../components/DashboadContainer.vue';
import WhiteList from '../components/WhiteList.vue';
import Limits from '../components/Limits.vue';
import Notifications from '../components/Notifications.vue';
import GeneralSettings from '../components/GeneralSettings.vue';
import About from '../components/About.vue';

const tabs = [
  'Dashboard',
  'Whitelist',
  'Limits',
  'Notifications',
  'General Settings',
  'About',
];
const activeTab = ref(tabs[0]);
</script>

<style scoped>
.all-in-one {
  font-family: sans-serif;
  padding: 2rem;
}
.tabs {
  display: flex;
  gap: 1rem;
  margin-bottom: 1.5rem;
}
.tabs button {
  background: #f5f5f5;
  border: none;
  padding: 0.5rem 1.2rem;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
}
.tabs button.active {
  background: #e0e0e0;
  font-weight: bold;
}
.tab-content {
  background: #fff;
  border-radius: 8px;
  padding: 1.5rem;
  min-height: 200px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.07);
}
</style> 