<template>
  <div class="demo">
    <h2>{{ translate('basic') }}</h2>
    <nut-address-list
      :data="data"
      :show-bottom-button="false"
      :data-options="dataOptions"
      @click-item="clickItem"
      @del-icon="delClick"
      @edit-icon="editClick"
    >
    </nut-address-list>
    <h2>{{ translate('title1') }}</h2>
    <nut-address-list
      :data="data"
      long-press
      :show-bottom-button="false"
      :data-options="dataOptions"
      @click-item="clickItem"
      @del-icon="delClick"
      @edit-icon="editClick"
      @long-copy="copyClick"
      @long-set="setClick"
      @long-del="delClickLong"
    >
    </nut-address-list>
    <h2>{{ translate('title2') }}</h2>
    <nut-address-list
      :data="data"
      swipe-edition
      show-bottom-button
      :data-options="dataOptions"
      @click-item="clickItem"
      @del-icon="delClick"
      @edit-icon="editClick"
      @swipe-del="delClickSwipe"
      @add="addAddress"
    >
    </nut-address-list>
  </div>
</template>

<script lang="ts">
import { createComponent } from '@/packages/utils/create';
import { reactive, ref, defineComponent } from 'vue';
const { translate } = createComponent('address-list');
import { useTranslate } from '@/sites/assets/util/useTranslate';
const initTranslate = () =>
  useTranslate({
    'zh-CN': {
      basic: '基础用法',
      title1: '长按功能',
      title2: '滑动功能'
    },
    'en-US': {
      basic: 'Basic Usage',
      title1: 'Long Press Function',
      title2: 'Swipe Function'
    }
  });
export default defineComponent({
  props: {},
  setup() {
    initTranslate();
    const data = ref([
      {
        testid: 3,
        testaddressName: '姓名',
        phone: '123****4567',
        defaultAddress: false,
        fullAddress: '北京市通州区测试测试测试测试测试测试测试测试测试'
      },
      {
        testid: 4,
        testaddressName: '姓名',
        phone: '123****4567',
        defaultAddress: true,
        fullAddress: '北京市通州区测试测试测试测试测试测试测试测试测试'
      }
    ]);
    const dataOptions = reactive({
      id: 'testid',
      addressDetail: 'testaddressDetail',
      addressName: 'testaddressName'
    });

    const clickItem = () => {
      console.log('Click To Address');
    };
    const delClick = () => {
      console.log('Click To Delete');
    };
    const editClick = () => {
      console.log('Click To Edit');
    };
    const copyClick = () => {
      console.log('Click To Copy');
    };
    const setClick = () => {
      console.log('Click On Settings');
    };
    const delClickLong = () => {
      console.log('Click On DelClickLong');
    };
    const delClickSwipe = () => {
      console.log('Click On DelClickSwipe');
    };
    const addAddress = () => {
      console.log('Click To Add');
    };
    return {
      data,
      clickItem,
      delClick,
      editClick,
      copyClick,
      setClick,
      delClickLong,
      delClickSwipe,
      addAddress,
      dataOptions,
      translate
    };
  }
});
</script>

<style lang="scss" scoped>
.demo {
  padding-right: 0 !important;
  padding-left: 0 !important;
}
</style>
