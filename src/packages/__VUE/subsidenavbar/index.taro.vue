<template>
  <view :class="classes" :ikey="ikey">
    <view class="nut-sub-side-navbar__title" @click.stop="handleClick">
      <span class="nut-sub-side-navbar__title__text">{{ title }}</span>
      <span class="nut-sub-side-navbar__title__icon">
        <ArrowDown2 v-if="!direction" size="12px"></ArrowDown2>
        <ArrowUp2 v-else size="12px"></ArrowUp2>
      </span>
    </view>
    <view
      v-show="!direction"
      class="nut-sub-side-navbar__list"
      :class="!direction ? 'nutFadeIn' : 'nutFadeOut'"
      :style="style"
    >
      <slot></slot>
    </view>
  </view>
</template>
<script lang="ts">
import { computed, onMounted, reactive, toRefs } from 'vue';
import { createComponent } from '@/packages/utils/create';
import { ArrowDown2, ArrowUp2 } from '@nutui/icons-vue-taro';
const { componentName, create } = createComponent('sub-side-navbar');
export default create({
  props: {
    title: {
      type: String,
      default: ''
    },
    ikey: {
      type: [String, Number],
      default: ''
    },
    open: {
      type: Boolean,
      default: true
    }
  },
  components: { ArrowDown2, ArrowUp2 },
  emits: ['titleClick'],
  setup: (props: any, context: any) => {
    const state = reactive({
      direction: ''
    });
    const classes = computed(() => {
      const prefixCls = componentName;
      return {
        [prefixCls]: true
      };
    });
    const style = computed(() => {
      return {
        height: !state.direction ? 'auto' : '0px'
      };
    });
    const handleClick = () => {
      context.emit('titleClick');
      state.direction = !state.direction ? 'up' : '';
    };
    onMounted(() => {
      state.direction = props.open ? '' : 'up';
    });
    return {
      ...toRefs(state),
      classes,
      style,
      handleClick
    };
  }
});
</script>
