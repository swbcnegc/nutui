<template>
  <view :class="classes">
    <view ref="barRef" class="nut-menu__bar" :class="{ opened: opened }">
      <template v-for="(item, index) in children" :key="index">
        <view
          class="nut-menu__item"
          :class="{ disabled: item.disabled, active: item.state.showPopup }"
          :style="{ color: item.state.showPopup ? activeColor : '' }"
          @click="!item.disabled && toggleItem(index)"
        >
          <view class="nut-menu__title" :class="getClasses(item.state.showPopup)">
            <view class="nut-menu__title-text">{{ item.renderTitle() }}</view>
            <span class="nut-menu__title-icon">
              <slot name="icon">
                <ArrowUp2 v-if="direction === 'up'" />
                <ArrowDown2 v-else />
              </slot>
            </span>
          </view>
        </view>
      </template>
    </view>
    <slot></slot>
  </view>
</template>
<script lang="ts">
import { reactive, provide, computed, ref, onMounted, onUnmounted } from 'vue';
import { createComponent } from '@/packages/utils/create';
import { useRect } from '@/packages/utils/useRect';
const { componentName, create } = createComponent('menu');
import { ArrowUp2, ArrowDown2 } from '@nutui/icons-vue';
export default create({
  components: {
    ArrowUp2,
    ArrowDown2
  },
  props: {
    activeColor: {
      type: String,
      default: ''
    },
    overlay: {
      type: Boolean,
      default: true as const
    },
    lockScroll: {
      type: Boolean,
      default: true as const
    },
    duration: {
      type: [Number, String],
      default: 0.3
    },
    closeOnClickOverlay: {
      type: Boolean,
      default: true
    },
    direction: {
      type: String,
      default: 'down'
    },
    scrollFixed: {
      type: [Boolean, String, Number],
      default: false
    },
    titleClass: [String]
  },
  setup(props) {
    const barRef = ref<HTMLElement>();
    const offset = ref(0);
    const isScrollFixed = ref(false);

    const useChildren = () => {
      const publicChildren: any[] = reactive([]);
      const internalChildren: any[] = reactive([]);

      const linkChildren = (value?: any) => {
        const link = (child: any) => {
          if (child.proxy) {
            internalChildren.push(child);
            publicChildren.push(child.proxy as any);
          }
        };

        const removeLink = (child: any) => {
          if (child.proxy) {
            let internalIndex = internalChildren.indexOf(child);
            if (internalIndex > -1) {
              internalChildren.splice(internalIndex, 1);
            }

            let publicIndex = publicChildren.indexOf(child.proxy);
            if (internalIndex > -1) {
              publicChildren.splice(publicIndex, 1);
            }
          }
        };

        provide(
          'menuParent',
          Object.assign(
            {
              removeLink,
              link,
              children: publicChildren,
              internalChildren
            },
            value
          )
        );
      };

      return {
        children: publicChildren,
        linkChildren
      };
    };

    const { children, linkChildren } = useChildren();

    const opened = computed(() => children.some((item) => item.state.showWrapper));

    const classes = computed(() => {
      const prefixCls = componentName;
      return {
        [prefixCls]: true,
        'scroll-fixed': isScrollFixed.value
      };
    });

    const updateOffset = () => {
      if (barRef.value) {
        const rect = useRect(barRef);

        if (props.direction === 'down') {
          offset.value = rect.bottom;
        } else {
          offset.value = window.innerHeight - rect.top;
        }
      }
    };

    linkChildren({ props, offset });

    const toggleItem = (active: number) => {
      children.forEach((item, index) => {
        if (index === active) {
          updateOffset();
          item.toggle();
        } else if (item.state.showPopup) {
          item.toggle(false, { immediate: true });
        }
      });
    };

    const getScrollTop = (el: Element | Window) => {
      return Math.max(0, 'scrollTop' in el ? el.scrollTop : el.pageYOffset);
    };

    const onScroll = () => {
      const { scrollFixed } = props;

      const scrollTop = getScrollTop(window);

      isScrollFixed.value = scrollTop > (typeof scrollFixed === 'boolean' ? 30 : Number(scrollFixed));
    };

    const getClasses = (showPopup: boolean) => {
      let str = '';
      const { titleClass } = props;

      if (showPopup) {
        str += 'active';
      }

      if (titleClass) {
        str += ` ${titleClass}`;
      }

      return str;
    };

    onMounted(() => {
      const { scrollFixed } = props;

      if (scrollFixed) {
        window.addEventListener('scroll', onScroll);
      }
    });

    onUnmounted(() => {
      const { scrollFixed } = props;

      if (scrollFixed) {
        window.removeEventListener('scroll', onScroll);
      }
    });

    return {
      toggleItem,
      children,
      opened,
      classes,
      barRef,
      getClasses
    };
  }
});
</script>
