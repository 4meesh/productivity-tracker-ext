<template>
  <div class="review-block" v-if="showReview">
    <p>{{ t('enjoyAndReview.message') }}</p>
    <span @click="closeBlock()" style="cursor:pointer;float:right;padding-top:9px;">[Close]</span>
    <input type="button" :value="t('enjoyAndReview.description')" @click="openStore()" />
  </div>
</template>

<script lang="ts">
export default {
  name: 'Review',
};
</script>

<script lang="ts" setup>
import { onMounted, ref } from 'vue';
import { useI18n } from 'vue-i18n';
import { injectStorage } from '../storage/inject-storage';
import { StorageParams } from '../storage/storage-params';
import { addDays, startOfToday } from 'date-fns';
import { addHours } from 'date-fns/esm';
import { CHROME_STORE_REVIEW_URL, EDGE_STORE_REVIEW_URL } from '../utils/chrome-url';

const { t } = useI18n();

const settingsStorage = injectStorage();
const PROMPT_AT_TIME_OF_DAY = 12;
const ADD_DAYS_FIRST = 2;
const ADD_DAYS_NEXT = 5;

const showReview = ref<boolean>();

onMounted(async () => {
  showReview.value = false;
  const reviewDate = await settingsStorage.getValue(StorageParams.REVIEW_DATE);

  if (reviewDate == undefined) {
    let nextTime = await settingsStorage.getValue(StorageParams.REVIEW_PROMPT_AT);
    if (nextTime == undefined) {
      await settingsStorage.saveValue(
        StorageParams.REVIEW_PROMPT_AT,
        addDays(addHours(startOfToday(), PROMPT_AT_TIME_OF_DAY), ADD_DAYS_FIRST).toString(),
      );
    } else {
      nextTime = new Date(nextTime);
      if (nextTime < new Date()) showReview.value = true;
    }
  }
});

async function closeBlock() {
  showReview.value = false;
  await settingsStorage.saveValue(
    StorageParams.REVIEW_PROMPT_AT,
    addDays(addHours(startOfToday(), PROMPT_AT_TIME_OF_DAY), ADD_DAYS_NEXT).toString(),
  );
}

async function openStore() {
  window.open(__BROWSER__ == 'edge' ? EDGE_STORE_REVIEW_URL : CHROME_STORE_REVIEW_URL, '_blank');
  await settingsStorage.saveValue(StorageParams.REVIEW_DATE, new Date().toString());
}
</script>

<style scoped>
.review-block {
  width: -webkit-fill-available;
  position: fixed;
  bottom: 0;
  padding: 8px 20px;
  font-size: 14px;
  background-color: #efefef;
}
.review-block input[type='button'] {
  margin: 0 20px 0 0;
  float: right;
  width: auto;
}
.review-block p {
  display: inline-block;
  margin: 8px;
  font-size: 17px;
  font-weight: 500;
}
</style>
