<template>
  <div class="demo">
    <h2>{{ translate('basic') }}</h2>
    <nut-cell class="danmu-box">
      <nut-barrage ref="danmu" :danmu="list"></nut-barrage>
    </nut-cell>
    <div class="test">
      <nut-button class="add nut-button--primary" @click="addDanmu">{{ translate('btn1') }}</nut-button>
    </div>
    <h2>{{ translate('slotTitle') }}</h2>
    <nut-cell class="danmu-box">
      <nut-barrage>
        <span>aaa</span>
        <span>bbb</span>
        <span>ccc</span>
        <span>ddd</span>
      </nut-barrage>
    </nut-cell>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from 'vue';
import { createComponent } from '@/packages/utils/create';
const { translate } = createComponent('barrage');
import { useTranslate } from '@/sites/assets/util/useTranslate';
const initTranslate = () =>
  useTranslate({
    'zh-CN': {
      basic: '基础用法',
      slotTitle: 'slot 用法',
      btn1: '随机添加'
    },
    'en-US': {
      basic: 'Basic Usage',
      slotTitle: 'slot usage',
      btn1: 'Random addition'
    }
  });
export default defineComponent({
  props: {},
  setup() {
    initTranslate();
    const inputVal = ref('');
    const danmu = ref();
    let list = ref(['画美不看', '不明觉厉', '喜大普奔', '男默女泪', '累觉不爱', '爷青结']);
    function addDanmu() {
      let n = Math.random();
      danmu.value.add('随机——' + String(n).substr(2, 10));
    }
    return {
      inputVal,
      danmu,
      list,
      addDanmu,
      translate
    };
  }
});
</script>

<style lang="scss" scoped>
.danmu-box,
.nut-barrage {
  padding: 20px 0;
  height: 150px;
}

.test {
  .add {
    display: block;
    padding: 5px 30px;
    margin: 20px auto;
    border-radius: 15px;
    font-size: 12px;
  }
}
</style>
