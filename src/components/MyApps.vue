<template>
  <div class="main">
    <p class="header">{{ t('tryMyOtherApps.message') }}</p>
    <div class="app-block" @click="openAppLink(App.ClearYoutube)">
      <div class="img-block">
        <span>[YouTube Logo]</span>
      </div>
      <div>
        <p class="title">{{ t('clearYoutube.message') }}</p>
        <p class="description">
          {{ t('clearYoutube.description') }}
        </p>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: 'MyApps',
};
</script>

<script lang="ts" setup>
import { useI18n } from 'vue-i18n';
import { CHROME_STORE_CLEAR_YOUTUBE_URL } from '../utils/chrome-url';
const { t } = useI18n();

enum App {
  ClearYoutube,
}

function openAppLink(app: App) {
  switch (app) {
    case App.ClearYoutube:
      window.open(CHROME_STORE_CLEAR_YOUTUBE_URL, '_blank');
      break;
  }
}
</script>

<style scoped>
.main {
  margin-top: 50px;
}
.header {
  font-size: 16px;
  font-weight: 500;
  text-transform: uppercase;
}
.app-block {
  margin: 20px 0;
  padding: 10px 20px;
  font-size: 14px;
  background-color: #efefef;
  border-radius: 10px;
  min-width: 655px;
  display: flex;
  flex-direction: row;
  gap: 20px;
  cursor: pointer;
}
.app-block .img-block {
  margin: auto 0;
}
.title {
  font-weight: 500;
  font-size: 15px;
}
.description {
  margin-top: 10px;
  font-size: 14px;
  color: black;
}
img {
  margin: auto;
}
</style>
