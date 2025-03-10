<template>
  <nut-popup
    v-model:visible="showPopup"
    :teleport="teleport"
    :close-on-click-overlay="false"
    :lock-scroll="lockScroll"
    :pop-class="popClass"
    :overlay-class="overlayClass"
    :overlay-style="overlayStyle"
    :style="popStyle"
    round
    @click-overlay="onClickOverlay"
    @click-close-icon="closed"
  >
    <view :class="classes">
      <view v-if="$slots.header || title" class="nut-dialog__header">
        <slot v-if="$slots.header" name="header"></slot>
        <template v-else>{{ title }}</template>
      </view>

      <view class="nut-dialog__content" :style="contentStyle">
        <slot v-if="$slots.default" name="default"></slot>
        <view v-else-if="typeof content === 'string'" v-html="content"></view>
        <component :is="content" v-else />
      </view>

      <view v-if="!noFooter" class="nut-dialog__footer" :class="{ [footerDirection]: footerDirection }">
        <slot v-if="$slots.footer" name="footer"></slot>
        <template v-else>
          <nut-button
            v-if="!noCancelBtn"
            size="small"
            plain
            type="primary"
            class="nut-dialog__footer-cancel"
            @click="onCancel"
          >
            {{ cancelText || translate('cancel') }}
          </nut-button>
          <nut-button v-if="!noOkBtn" size="small" type="primary" class="nut-dialog__footer-ok" @click="onOk">
            {{ okText || translate('confirm') }}
          </nut-button>
        </template>
      </view>
    </view>
  </nut-popup>
</template>
<script lang="ts">
import { onMounted, computed, watch, ref, PropType, VNode, CSSProperties } from 'vue';
import { createComponent } from '@/packages/utils/create';
const { componentName, create, translate } = createComponent('dialog');
import { funInterceptor, Interceptor } from '@/packages/utils/util';
import { popupProps } from '../popup/props';
import Popup from '../popup/index.vue';
import Button from '../button/index.vue';
export type TextAlign = 'left' | 'center' | 'right' | 'top';
export default create({
  inheritAttrs: false,
  components: {
    [Popup.name]: Popup,
    [Button.name]: Button
  },
  props: {
    ...popupProps,
    closeOnClickOverlay: {
      type: Boolean,
      default: true
    },
    title: {
      type: String,
      default: ''
    },
    content: {
      type: [String, Object] as PropType<string | VNode>,
      default: ''
    },
    noFooter: {
      type: Boolean,
      default: false
    },
    noOkBtn: {
      type: Boolean,
      default: false
    },
    noCancelBtn: {
      type: Boolean,
      default: false
    },
    cancelText: {
      type: String,
      default: ''
    },
    okText: {
      type: String,
      default: ''
    },
    cancelAutoClose: {
      type: Boolean,
      default: true
    },
    okAutoClose: {
      type: Boolean,
      default: true
    },
    textAlign: {
      type: String as PropType<TextAlign>,
      default: 'center'
    },
    closeOnPopstate: {
      type: Boolean,
      default: false
    },
    footerDirection: {
      type: String,
      default: 'horizontal' //vertical
    },
    customClass: {
      type: String,
      default: ''
    },
    popStyle: {
      type: Object as PropType<CSSProperties>
    },
    beforeClose: Function as PropType<Interceptor>
  },
  emits: ['update', 'update:visible', 'ok', 'cancel', 'opened', 'closed'],
  setup(props, { emit }) {
    const showPopup = ref(props.visible);
    onMounted(() => {
      if (props.closeOnPopstate) {
        window.addEventListener('popstate', function () {
          closed('page');
        });
      }
    });

    watch(
      () => props.visible,
      (value) => {
        showPopup.value = value;
        if (value) {
          emit('opened');
        }
      }
    );

    const classes = computed(() => {
      return {
        [componentName]: true,
        [props.customClass]: true
      };
    });

    const update = (val: boolean) => {
      emit('update', val);
      emit('update:visible', val);
    };

    const closed = (action: string) => {
      funInterceptor(props.beforeClose, {
        args: [action],
        done: () => {
          showPopup.value = false;
          update(false);
          emit('closed');
        }
      });
    };

    const onCancel = () => {
      emit('cancel');
      if (props.cancelAutoClose) {
        showPopup.value = false;
        closed('cancel');
      }
    };

    const onOk = () => {
      emit('ok');
      if (props.okAutoClose) {
        closed('ok');
      }
    };

    const onClickOverlay = () => {
      if (props.closeOnClickOverlay) {
        closed('');
      }
    };

    const contentStyle = computed(() => {
      return {
        textAlign: props.textAlign
      } as CSSProperties;
    });

    return {
      closed,
      classes,
      onCancel,
      onOk,
      showPopup,
      onClickOverlay,
      contentStyle,
      translate
    };
  }
});
</script>
