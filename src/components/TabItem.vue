<template>
  <div class="tab-item">
    <Favicon :favicon="item.favicon" :type="typeOfUrl" />
    <div
      class="ml-10 flex-grow-2"
      @mouseover="isShowCmdButtons = true"
      @mouseleave="isShowCmdButtons = false"
    >
      <div class="first-block">
        <div>
          <p class="url">{{ url }}</p>
          <BadgeIcons :url="url" :type="typeOfUrl" :listType="listType" />
          <p class="links" v-if="isShowCmdButtons" title="Statistics">
            <span>[Details Link]</span>
          </p>

          <p class="links" v-if="isShowCmdButtons" title="Open website">
            <span>[Open Link]</span>
          </p>
        </div>
        <p class="text-right time">{{ summaryTimeForTab }}</p>
      </div>
      <p v-if="showWarningMessage" class="warning-message">
        {{ t('cannotOpenFile.message') }}
      </p>
      <div class="second-block">
        <div class="progress-bar">
          <div :style="styleForProgressBar"></div>
        </div>
        <p class="text-right percent">{{ percent }} %</p>
      </div>
      <p class="sessions">{{ sessions }}</p>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: 'TabItem',
};
</script>

<script lang="ts" setup>
import { computed, ref } from 'vue';
import { useI18n } from 'vue-i18n';
import Favicon from './Favicon.vue';
import BadgeIcons from './BadgeIcons.vue';
import { convertSummaryTimeToString } from '../utils/converter';
import { getPercentage } from '../utils/common';
import { CurrentTabItem } from '../dto/currentTabItem';
import { SettingsTab, TypeOfList, TypeOfUrl } from '../utils/enums';
import { openPage } from '../utils/open-page';
import { getTypeOfUrl } from '../utils/get-type-of-url';

const { t } = useI18n();

const props = defineProps<{
  item: CurrentTabItem;
  summaryTimeForWholeDay: number;
  listType: TypeOfList;
}>();

const isShowCmdButtons = ref<boolean>();

const typeOfUrl = computed(() => getTypeOfUrl(props.item.url));

const url = computed(() =>
  typeOfUrl.value == TypeOfUrl.Document
    ? decodeURI(props.item.url.split('///')[1])
    : props.item.url,
);

const sessions = computed(() => {
  if (props.item.sessions == 0) return `0 ${t('someSession.message')}`;
  if (props.item.sessions > 1) return `${props.item.sessions} ${t('someSession.message')}`;
  if (props.item.sessions == 1) return `${props.item.sessions} ${t('session.message')}`;
});

const summaryTimeForTab = computed(() => convertSummaryTimeToString(props.item.summaryTime));
const percent = computed(() => getPercentage(props.item.summaryTime, props.summaryTimeForWholeDay));

const styleForProgressBar = computed(() => `width: ${percent.value}%`);

function openUrl(url: string) {
  if (typeOfUrl.value != TypeOfUrl.Document && !url.startsWith('http')) {
    url = `https://${url}`;
    window.open(url, '_blank');
  } else showWarningMessage.value = true;
}

async function openStats(url: string) {
  await openPage(SettingsTab.WebsiteStats, url);
}

const showWarningMessage = ref<boolean>();
</script>

<style scoped>
.tab-item {
  padding: 7px;
  border: 1px transparent solid;
  border-radius: 10px;
  margin: 5px 15px;
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
.tab-item:hover {
  border: 1px rgb(202, 202, 202) solid;
}

.tab-item .links {
  display: inline-block;
  margin: 0;
  cursor: pointer;
  margin: 0 5px;
}
.tab-item .links .link {
  vertical-align: middle;
}
.tab-item .url {
  font-size: 15px;
  cursor: pointer;
  overflow-wrap: anywhere;
  display: inline-block;
}
.tab-item .url:hover {
  color: rgb(99, 99, 243);
}
.tab-item p {
  margin: 5px;
}
.tab-item .time {
  font-size: 14px;
  font-weight: 600;
}
.tab-item .progress-bar {
  width: 100%;
  margin: 5px 0 0 5px;
  border-radius: 10px;
  border: 1.5px rgb(225 224 224) solid;
}
.tab-item .progress-bar div {
  height: 6px;
  background-color: var(--progress-bar);
}
.flex-grow-2 {
  flex-grow: 2;
}
.tab-item .first-block {
  display: flex;
  justify-content: space-between;
}
.tab-item .second-block {
  display: flex;
  flex-direction: row;
  align-items: baseline;
}
.tab-item .percent {
  white-space: nowrap;
  margin: 0 5px 0 20px;
}
.tab-item .sessions {
  margin: 0 0 0 5px;
}
.tab-item .warning-message {
  color: grey;
}
</style>
