<template>
  <template v-if="poppable">
    <nut-popup
      v-model:visible="innerVisible"
      position="bottom"
      pop-class="nut-cascader__popup"
      round
      :closeable="closeable"
      :close-icon="closeIcon"
      :destroy-on-close="false"
      :close-icon-position="closeIconPosition"
      :lock-scroll="lockScroll"
      :catch-move="lockScroll"
    >
      <template v-if="title">
        <view class="nut-cascader__bar" v-html="title"></view>
      </template>

      <nut-cascader-item
        :model-value="innerValue"
        :options="options"
        :lazy="lazy"
        :lazy-load="lazyLoad"
        :value-key="valueKey"
        :text-key="textKey"
        :children-key="childrenKey"
        :convert-config="convertConfig"
        :visible="innerVisible"
        @change="onChange"
        @path-change="onPathChange"
      />
    </nut-popup>
  </template>

  <template v-else>
    <nut-cascader-item
      :model-value="innerValue"
      :options="options"
      :lazy="lazy"
      :lazy-load="lazyLoad"
      :value-key="valueKey"
      :text-key="textKey"
      :children-key="childrenKey"
      :convert-config="convertConfig"
      :visible="innerVisible"
      @change="onChange"
      @path-change="onPathChange"
    />
  </template>
</template>
<script lang="ts">
import { watch, ref, Ref, computed } from 'vue';
import { CascaderValue, CascaderOption } from './types';
import { createComponent } from '@/packages/utils/create';
import { popupProps } from '../popup/props';
const { create } = createComponent('cascader');
import CascaderItem from './cascader-item.taro.vue';
import Popup from '../popup/index.taro.vue';

export default create({
  components: {
    [CascaderItem.name]: CascaderItem,
    [Popup.name]: Popup
  },
  props: {
    ...popupProps,
    modelValue: Array,
    visible: Boolean,
    title: String,
    options: {
      type: Array,
      default: () => []
    },
    lazy: Boolean,
    lazyLoad: Function,
    valueKey: {
      type: String,
      default: 'value'
    },
    textKey: {
      type: String,
      default: 'text'
    },
    childrenKey: {
      type: String,
      default: 'children'
    },
    poppable: {
      type: Boolean,
      default: true
    },
    convertConfig: Object
  },
  emits: ['update:modelValue', 'change', 'pathChange', 'update:visible'],
  setup(props, { emit }) {
    const innerValue: Ref<CascaderValue> = ref(props.modelValue as CascaderValue);
    const innerVisible = computed({
      get() {
        return props.visible;
      },
      set(value) {
        emit('update:visible', value);
      }
    });

    const onChange = (value: CascaderValue, pathNodes: CascaderOption[]) => {
      innerValue.value = value;
      innerVisible.value = false;
      emit('change', value, pathNodes);
      emit('update:modelValue', value);
    };

    const onPathChange = (pathNodes: CascaderOption[]) => {
      emit('pathChange', pathNodes);
    };

    watch(
      () => props.modelValue,
      (value) => {
        if (value !== innerValue.value) {
          innerValue.value = value as CascaderValue;
        }
      }
    );

    return {
      onChange,
      onPathChange,
      innerValue,
      innerVisible
    };
  }
});
</script>
