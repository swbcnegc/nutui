<template>
  <view class="nut-sku-header">
    <image v-if="ENV != ENV_TYPE.WEB" class="nut-sku-header-img" :src="goods.imagePath" />
    <img v-else class="nut-sku-header-img" :src="goods.imagePath" />
    <view class="nut-sku-header-right">
      <template v-if="getSlots('sku-header-price')">
        <slot name="sku-header-price"></slot>
      </template>
      <nut-price v-else :price="goods.price" :need-symbol="true" :thousands="false"> </nut-price>

      <template v-if="getSlots('sku-header-extra')">
        <slot name="sku-header-extra"></slot>
      </template>
      <view v-if="goods.skuId && !getSlots('sku-header-extra')" class="nut-sku-header-right-extra"
        >{{ translate('skuId') }}&nbsp;:&nbsp;{{ goods.skuId }}</view
      >
    </view>
  </view>
</template>
<script lang="ts">
import { reactive, toRefs } from 'vue';
import Price from '../../price/index.taro.vue';
import { createComponent } from '@/packages/utils/create';
import Taro from '@tarojs/taro';
const { create, translate } = createComponent('sku-header');

export default create({
  props: {
    goods: {
      type: Object,
      default: {}
    }
  },
  components: {
    [Price.name]: Price
  },
  setup(props, { slots }) {
    const getSlots = (name: string) => slots[name];

    const state = reactive({
      ENV: Taro.getEnv(),
      ENV_TYPE: Taro.ENV_TYPE
    });

    return {
      getSlots,
      translate,
      ...toRefs(state)
    };
  }
});
</script>
