<template>
  <div class="demo">
    <h2>{{ translate('basic') }}</h2>
    <nut-cell>
      <nut-trend-arrow :sync-text-color="false" :rate="1" />
      <nut-trend-arrow :sync-text-color="false" :rate="-0.2535" />
    </nut-cell>
    <view class="title">{{ translate('title1') }}</view>
    <nut-cell>
      <nut-trend-arrow :rate="10.2365" />
      <nut-trend-arrow :rate="-0.2535" />
    </nut-cell>
    <view class="title">{{ translate('title2') }}</view>
    <nut-cell>
      <nut-trend-arrow :digits="1" :rate="10.2365" />
      <nut-trend-arrow :digits="3" :rate="-0.2535" />
    </nut-cell>
    <view class="title">{{ translate('title3') }}</view>
    <nut-cell>
      <nut-trend-arrow arrow-left :rate="0.2535" />
      <nut-trend-arrow arrow-left :rate="-0.2535" />
    </nut-cell>
    <view class="title">{{ translate('title4') }}</view>
    <nut-cell>
      <nut-trend-arrow show-sign :rate="1" />
      <nut-trend-arrow show-sign :rate="-0.2535" />
    </nut-cell>
    <view class="title">{{ translate('title5') }}</view>
    <nut-cell>
      <nut-trend-arrow show-sign :rate="0" />
      <nut-trend-arrow show-sign show-zero :rate="0" />
    </nut-cell>
    <view class="title">{{ translate('title6') }}</view>
    <nut-cell>
      <nut-trend-arrow :rate="10.2365" rise-color="rgb(73,143,242)" />
      <nut-trend-arrow :rate="-0.2535" show-sign drop-color="rgb(255, 190, 13)" />
      <nut-trend-arrow
        :sync-text-color="false"
        show-sign
        :rate="-0.2535"
        text-color="rgb(39,197,48)"
        drop-color="rgb(255, 190, 13)"
      />
    </nut-cell>
    <view class="title">{{ translate('title7') }}</view>
    <nut-cell>
      <nut-trend-arrow :rate="10.2365">
        <template #up-icon>
          <Success color="blue" width="18" height="18" />
        </template>
      </nut-trend-arrow>
      <nut-trend-arrow :rate="-10.2365">
        <template #down-icon>
          <Failure color="red" />
        </template>
      </nut-trend-arrow>
    </nut-cell>
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import { createComponent } from '@/packages/utils/create';
const { translate } = createComponent('trend-arrow');
import { useTranslate } from '@/sites/assets/util/useTranslate';
import { Success, Failure } from '@nutui/icons-vue';
const initTranslate = () =>
  useTranslate({
    'zh-CN': {
      basic: '基础用法',
      title1: '改变文字颜色',
      title2: '指定小数位',
      title3: '箭头在前面',
      title4: '显示正负号',
      title5: '是否展示0',
      title6: '自定义颜色',
      title7: '自定义图标'
    },
    'en-US': {
      basic: 'Basic Usage',
      title1: 'Change text color',
      title2: 'Specify decimal places',
      title3: 'Arrow ahead',
      title4: 'show sign',
      title5: 'Whether to show 0',
      title6: 'Custom color',
      title7: 'Custom icon'
    }
  });
export default defineComponent({
  components: { Success, Failure },
  props: {},
  setup() {
    initTranslate();
    return { translate };
  }
});
</script>
<style lang="scss" scoped>
.demo {
  .title {
    font-size: 14px;
    color: rgb(144, 156, 164);
    padding: 0 10px;
    font-weight: normal;
  }

  .nut-trend-arrow {
    margin-right: 10px;
  }
}
</style>
