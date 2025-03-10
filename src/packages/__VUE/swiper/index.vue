<template>
  <view
    ref="container"
    :class="classes"
    @touchstart="onTouchStart"
    @touchmove="onTouchMove"
    @touchend="onTouchEnd"
    @touchcancel="onTouchEnd"
  >
    <view :class="classesInner" :style="state.style">
      <slot></slot>
    </view>
    <slot name="page"></slot>
    <view v-if="paginationVisible && !$slots.page" :class="classesPagination">
      <i
        v-for="(item, index) in state.children.length"
        :key="index"
        :style="{
          backgroundColor: activePagination === index ? paginationColor : '#ddd'
        }"
        :class="{ active: activePagination === index }"
      />
    </view>
  </view>
</template>

<script lang="ts">
import {
  onDeactivated,
  onBeforeUnmount,
  provide,
  ComponentInternalInstance,
  ComponentPublicInstance,
  reactive,
  computed,
  nextTick,
  ref,
  watch,
  VNode
} from 'vue';
import { createComponent } from '@/packages/utils/create';
import { useTouch } from '@/packages/utils/useTouch/index';
import { useExpose } from '@/packages/utils/useExpose/index';
import { clamp } from '@/packages/utils/util';
import requestAniFrame from '@/packages/utils/raf';
const { create, componentName } = createComponent('swiper');
export default create({
  props: {
    width: {
      type: [Number, String],
      default: window.innerWidth
    },
    height: {
      type: [Number, String],
      default: 0
    },
    direction: {
      type: String,
      default: 'horizontal' //horizontal and vertical
    },
    paginationVisible: {
      type: Boolean,
      default: false
    },
    paginationColor: {
      type: String,
      default: '#fff'
    },
    loop: {
      type: Boolean,
      default: true
    },
    duration: {
      type: [Number, String],
      default: 500
    },
    autoPlay: {
      type: [Number, String],
      default: 0
    },
    initPage: {
      type: [Number, String],
      default: 0
    },
    touchable: {
      type: Boolean,
      default: true
    },
    isPreventDefault: {
      type: Boolean,
      default: true
    },
    isStopPropagation: {
      type: Boolean,
      default: true
    }
  },
  emits: ['change'],

  setup(props, { emit, slots }) {
    const container = ref();
    const state = reactive({
      active: 0,
      num: 0,
      rect: null as DOMRect | null,
      width: 0,
      height: 0,
      moving: false,
      offset: 0,
      touchTime: 0,
      autoplayTimer: null as NodeJS.Timeout | undefined | null,
      children: [] as ComponentPublicInstance[],
      childrenVNode: [] as VNode[],
      style: {}
    });

    const touch = useTouch();

    const classes = computed(() => {
      const prefixCls = componentName;
      return {
        [prefixCls]: true
      };
    });

    const isVertical = computed(() => props.direction === 'vertical');

    const classesInner = computed(() => {
      const prefixCls = componentName;
      return {
        [`${prefixCls}-inner`]: true,
        [`${prefixCls}-vertical`]: isVertical.value
      };
    });

    const classesPagination = computed(() => {
      const prefixCls = componentName;
      return {
        [`${prefixCls}-pagination`]: true,
        [`${prefixCls}-pagination-vertical`]: isVertical.value
      };
    });

    const delTa = computed(() => {
      return isVertical.value ? touch.deltaY.value : touch.deltaX.value;
    });

    const isCorrectDirection = computed(() => {
      return touch.direction.value === props.direction;
    });

    const childCount = computed(() => state.children.length);

    const size = computed(() => state[isVertical.value ? 'height' : 'width']);

    const trackSize = computed(() => childCount.value * size.value);

    const minOffset = computed(() => {
      if (state.rect) {
        const base = isVertical.value ? state.rect.height : state.rect.width;
        return base - size.value * childCount.value;
      }
      return 0;
    });

    const activePagination = computed(() => (state.active + childCount.value) % childCount.value);

    const getStyle = () => {
      let offset = 0;
      offset = state.offset;
      state.style = {
        transitionDuration: `${state.moving ? 0 : props.duration}ms`,
        transform: `translate${isVertical.value ? 'Y' : 'X'}(${offset}px)`,
        [isVertical.value ? 'height' : 'width']: `${size.value * childCount.value}px`,
        [isVertical.value ? 'width' : 'height']: `${isVertical.value ? state.width : state.height}px`
      };
    };

    const relation = (child: ComponentInternalInstance) => {
      let children = [] as VNode[];
      const childrenVNodeLen = state.childrenVNode.length;
      let slot = slots?.default?.() as VNode[];
      slot = slot.filter((item: VNode) => item.children && Array.isArray(item.children));
      slot.forEach((item: VNode) => {
        children = children.concat(item.children as VNode[]);
      });
      if (!childrenVNodeLen) {
        state.childrenVNode = children.slice();
        child.proxy && state.children.push(child.proxy);
      } else {
        if (childrenVNodeLen > children.length) {
          state.children = state.children.filter((item: ComponentPublicInstance) => child.proxy !== item);
        } else if (childrenVNodeLen < children.length) {
          for (let i = 0; i < childrenVNodeLen; i++) {
            if ((children[i] as VNode).key !== (state.childrenVNode[i] as VNode).key) {
              child.proxy && state.children.splice(i, 0, child.proxy);
              child.vnode && state.childrenVNode.splice(i, 0, child.vnode);
              break;
            }
          }
          if (childrenVNodeLen !== children.length) {
            child.proxy && state.children.push(child.proxy);
            child.vnode && state.childrenVNode.push(child.vnode);
          }
        } else {
          state.childrenVNode = children.slice();
          child.proxy && state.children.push(child.proxy);
        }
      }
    };

    const getOffset = (active: number, offset = 0) => {
      let currentPosition = active * size.value;
      if (!props.loop) {
        currentPosition = Math.min(currentPosition, -minOffset.value);
      }

      let targetOffset = offset - currentPosition;
      if (!props.loop) {
        targetOffset = clamp(targetOffset, minOffset.value, 0);
      }

      // console.log(offset, currentPosition, targetOffset);

      return targetOffset;
    };

    const getActive = (pace: number) => {
      const { active } = state;
      if (pace) {
        if (props.loop) {
          return clamp(active + pace, -1, childCount.value);
        }
        return clamp(active + pace, 0, childCount.value - 1);
      }
      return active;
    };

    const move = ({ pace = 0, offset = 0, isEmit = false }) => {
      if (childCount.value <= 1) return;

      const { active } = state;

      const targetActive = getActive(pace);
      const targetOffset = getOffset(targetActive, offset);

      if (props.loop) {
        if (state.children[0] && targetOffset !== minOffset.value) {
          const rightBound = targetOffset < minOffset.value;
          (state.children[0] as any).setOffset(rightBound ? trackSize.value : 0);
        }
        if (state.children[childCount.value - 1] && targetOffset !== 0) {
          const leftBound = targetOffset > 0;
          (state.children[childCount.value - 1] as any).setOffset(leftBound ? -trackSize.value : 0);
        }
      }

      state.active = targetActive;
      state.offset = targetOffset;

      if (isEmit && active !== state.active) {
        emit('change', activePagination.value);
      }

      getStyle();
    };

    const resettPosition = () => {
      state.moving = true;

      if (state.active <= -1) {
        move({ pace: childCount.value });
      }
      if (state.active >= childCount.value) {
        move({ pace: -childCount.value });
      }
    };

    const stopAutoPlay = () => {
      if (state.autoplayTimer) {
        clearTimeout(state.autoplayTimer);
      }
    };

    const jump = (pace: number) => {
      resettPosition();
      touch.reset();

      requestAniFrame(() => {
        requestAniFrame(() => {
          state.moving = false;
          move({
            pace,
            isEmit: true
          });
        });
      });
    };

    const prev = () => {
      jump(-1);
    };

    const next = () => {
      jump(1);
    };

    const to = (index: number) => {
      resettPosition();

      touch.reset();

      requestAniFrame(() => {
        state.moving = false;
        let targetIndex;
        if (props.loop && childCount.value === index) {
          targetIndex = state.active === 0 ? 0 : index;
        } else {
          targetIndex = index % childCount.value;
        }
        move({
          pace: targetIndex - state.active,
          isEmit: true
        });
      });
    };

    const autoplay = () => {
      if (+props.autoPlay <= 0 || childCount.value <= 1) return;
      stopAutoPlay();

      state.autoplayTimer = setTimeout(() => {
        next();
        autoplay();
      }, Number(props.autoPlay));
    };

    const init = (active: number = +props.initPage) => {
      stopAutoPlay();
      state.rect = container.value.getBoundingClientRect();
      active = Math.min(childCount.value - 1, active);
      state.width = props.width ? +props.width : (state.rect as DOMRect).width;
      state.height = props.height ? +props.height : (state.rect as DOMRect).height;
      state.active = active;
      state.offset = getOffset(state.active);
      state.moving = true;
      getStyle();

      autoplay();
    };

    const onTouchStart = (e: TouchEvent) => {
      if (props.isPreventDefault) e.preventDefault();
      if (props.isStopPropagation) e.stopPropagation();
      if (!props.touchable) return;
      touch.start(e);
      state.touchTime = Date.now();
      stopAutoPlay();
      resettPosition();
    };

    const onTouchMove = (e: TouchEvent) => {
      if (props.touchable && state.moving) {
        touch.move(e);
        if (isCorrectDirection.value) {
          move({
            offset: delTa.value
          });
        }
      }
    };

    const onTouchEnd = () => {
      if (!props.touchable || !state.moving) return;
      const speed = delTa.value / (Date.now() - state.touchTime);
      const isShouldMove = Math.abs(speed) > 0.3 || Math.abs(delTa.value) > +(size.value / 2).toFixed(2);

      if (isShouldMove && isCorrectDirection.value) {
        let pace = 0;
        const offset = isVertical.value ? touch.offsetY.value : touch.offsetX.value;
        if (props.loop) {
          pace = offset > 0 ? (delTa.value > 0 ? -1 : 1) : 0;
        } else {
          pace = -Math[delTa.value > 0 ? 'ceil' : 'floor'](delTa.value / size.value);
        }
        move({
          pace,
          isEmit: true
        });
      } else if (delTa.value) {
        move({ pace: 0 });
      }
      state.moving = false;
      getStyle();
      autoplay();
    };

    provide('parent', {
      props,
      size,
      relation
    });

    useExpose({
      prev,
      next,
      to
    });

    onDeactivated(() => {
      stopAutoPlay();
    });

    onBeforeUnmount(() => {
      stopAutoPlay();
    });

    watch(
      () => props.initPage,
      (val) => {
        nextTick(() => {
          init(Number(val));
        });
      }
    );

    watch(
      () => props.height,
      () => {
        nextTick(() => {
          init();
        });
      }
    );

    watch(
      () => state.children.length,
      () => {
        nextTick(() => {
          init();
        });
      }
    );

    watch(
      () => props.autoPlay,
      (val) => {
        +val > 0 ? autoplay() : stopAutoPlay();
      }
    );

    return {
      state,
      classes,
      classesInner,
      classesPagination,
      container,
      activePagination,
      onTouchStart,
      onTouchMove,
      onTouchEnd
    };
  }
});
</script>
