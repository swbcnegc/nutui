<template>
  <div class="demo">
    <nut-cell-group :title="translate('title1')">
      <nut-cell>
        <nut-price :price="0" size="small" :need-symbol="false" />
      </nut-cell>
      <nut-cell>
        <nut-price :price="0" size="normal" :need-symbol="false" />
      </nut-cell>
      <nut-cell>
        <nut-price :price="0" size="large" :need-symbol="false" />
      </nut-cell>
    </nut-cell-group>
    <h2>{{ translate('title2') }}</h2>
    <nut-cell>
      <nut-price :price="8888" :decimal-digits="0" />
    </nut-cell>
    <h2>{{ translate('titleStrike') }}</h2>
    <nut-cell>
      <nut-price :price="8888" :decimal-digits="0" size="normal" need-symbol thousands strike-through />
    </nut-cell>
    <h2>{{ translate('title3') }}</h2>
    <nut-cell>
      <nut-price :price="10010.01" symbol="¥" />
    </nut-cell>
    <h2>{{ translate('title4') }}</h2>
    <nut-cell>
      <nut-price :price="8888.01" position="after" :symbol="translate('symbol')" />
    </nut-cell>
    <h2>{{ translate('title5') }}</h2>
    <nut-cell>
      <nut-price :price="15213.1221" :decimal-digits="3" thousands />
    </nut-cell>
  </div>
</template>

<script setup lang="ts">
import { createComponent } from '@/packages/utils/create';
const { translate } = createComponent('price');
import { useTranslate } from '@/sites/assets/util/useTranslate';
const initTranslate = () =>
  useTranslate({
    'zh-CN': {
      title1: '支持三种尺寸：small、normal、large',
      title2: '不保留小数',
      title3: '货币符号',
      title4: '货币符号位置',
      titleStrike: '划线价',
      title5: '千位分隔',
      symbol: '元'
    },
    'en-US': {
      title1: 'Support three sizes：small、normal、large',
      title2: 'No decimals',
      title3: 'Currency symbol',
      title4: 'Currency symbol position',
      titleStrike: 'Strike Through',
      title5: 'Separated by thousands',
      symbol: 'RMB'
    }
  });
initTranslate();
</script>
