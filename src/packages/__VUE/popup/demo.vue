<template>
  <div class="demo">
    <div id="test"></div>
    <h2>{{ translate('basic') }}</h2>
    <nut-cell :title="translate('cell1')" is-link @click="showBasic = true"></nut-cell>
    <nut-popup v-model:visible="showBasic" pop-class="popclass" :style="{ padding: '30px 50px' }" :z-index="100">
      {{ translate('text') }}
    </nut-popup>

    <h2>{{ translate('position') }}</h2>
    <nut-cell :title="translate('cell2')" is-link @click="showTop = true"></nut-cell>
    <nut-popup v-model:visible="showTop" position="top" :style="{ height: '20%' }"></nut-popup>

    <nut-cell :title="translate('cell3')" is-link @click="showBottom = true"></nut-cell>
    <nut-popup v-model:visible="showBottom" position="bottom" :style="{ height: '20%' }"></nut-popup>

    <nut-cell :title="translate('cell4')" is-link @click="showLeft = true"></nut-cell>
    <nut-popup v-model:visible="showLeft" position="left" :style="{ width: '20%', height: '100%' }"></nut-popup>

    <nut-cell :title="translate('cell5')" is-link @click="showRight = true"></nut-cell>
    <nut-popup v-model:visible="showRight" position="right" :style="{ width: '20%', height: '100%' }"></nut-popup>

    <h2>{{ translate('close') }}</h2>
    <nut-cell :title="translate('close')" is-link @click="showIcon = true"></nut-cell>
    <nut-popup v-model:visible="showIcon" position="bottom" closeable :style="{ height: '20%' }"></nut-popup>

    <nut-cell :title="translate('iposition')" is-link @click="showIconPosition = true"></nut-cell>
    <nut-popup
      v-model:visible="showIconPosition"
      position="bottom"
      closeable
      close-icon-position="top-left"
      :style="{ height: '20%' }"
    ></nut-popup>

    <nut-cell :title="translate('cicon')" is-link @click="showCloseIcon = true"></nut-cell>
    <nut-popup
      v-model:visible="showCloseIcon"
      position="bottom"
      closeable
      close-icon-position="top-left"
      :style="{ height: '20%' }"
    >
      <template #close-icon>
        <Heart></Heart>
      </template>
    </nut-popup>

    <h2>{{ translate('circle') }}</h2>
    <nut-cell :title="translate('circle')" is-link @click="showRound = true"></nut-cell>
    <nut-popup v-model:visible="showRound" position="bottom" closeable round :style="{ height: '30%' }"></nut-popup>

    <h2>{{ translate('teleport') }}</h2>
    <nut-cell :title="translate('teleport')" is-link @click="showTeleport = true"></nut-cell>
    <nut-popup v-model:visible="showTeleport" :style="{ padding: '30px 50px' }" teleport="#app" teleport-disable>
      app
    </nut-popup>

    <h2>{{ translate('muti') }}</h2>
    <nut-cell :title="translate('muti')" is-link @click="showPop1 = true"></nut-cell>
    <nut-popup v-model:visible="showPop1" :style="{ padding: '30px 50px' }">
      <div @click="showPop2 = true">{{ translate('click') }}</div>
    </nut-popup>
    <nut-popup v-model:visible="showPop2" :style="{ padding: '30px 50px' }">{{ translate('text') }}</nut-popup>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import NutPopup from './index.vue';
import NutCell from '../cell/index.vue';
import { createComponent } from '@/packages/utils/create';
const { translate } = createComponent('popup');
import { useTranslate } from '@/sites/assets/util/useTranslate';
import { Heart } from '@nutui/icons-vue';
const initTranslate = () =>
  useTranslate({
    'zh-CN': {
      basic: '基础用法',
      cell1: '展示弹出层',
      text: '正文',
      position: '弹出位置',
      cell2: '顶部弹出',
      cell3: '底部弹出',
      cell4: '左侧弹出',
      cell5: '右侧弹出',
      close: '关闭图标',
      iposition: '图标位置',
      cicon: '自定义图标',
      circle: '圆角弹框',
      teleport: '指定挂载节点',
      muti: '多层堆叠',
      click: '点击它'
    },
    'en-US': {
      basic: 'Basic Usage',
      cell1: 'Show pop-up layer',
      text: 'text',
      position: 'Eject position',
      cell2: 'Top pop-up',
      cell3: 'Bottom pop-up',
      cell4: 'Left pop-up',
      cell5: 'Right pop-up',
      close: 'Close icon',
      iposition: 'Icon location',
      cicon: 'Custom icon',
      circle: 'Rounded bullet frame',
      teleport: 'Specify mount node',
      muti: 'Multi stack',
      click: 'Click it'
    }
  });
initTranslate();
const showBasic = ref(false);
const showTop = ref(false);
const showBottom = ref(false);
const showLeft = ref(false);
const showRight = ref(false);
const showIcon = ref(false);
const showIconPosition = ref(false);
const showCloseIcon = ref(false);
const showRound = ref(false);
const showPop1 = ref(false);
const showPop2 = ref(false);
const showTeleport = ref(false);
</script>
