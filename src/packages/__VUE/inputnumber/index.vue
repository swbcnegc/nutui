<template>
  <view :class="classes">
    <view
      class="nut-input-number__icon nut-input-number__left"
      :class="{ 'nut-input-number__icon--disabled': !reduceAllow() }"
      @click="reduce"
    >
      <slot name="left-icon">
        <Minus :width="pxCheck(buttonSize)" :height="pxCheck(buttonSize)" />
      </slot>
    </view>
    <input
      type="number"
      :min="min"
      :max="max"
      :style="{ width: pxCheck(inputWidth), height: pxCheck(buttonSize) }"
      :disabled="disabled"
      :readonly="readonly"
      :value="modelValue"
      @input="change"
      @blur="blur"
      @focus="focus"
    />
    <view
      class="nut-input-number__icon nut-input-number__right"
      :class="{ 'nut-input-number__icon--disabled': !addAllow() }"
      @click="add"
    >
      <slot name="right-icon">
        <Plus :width="pxCheck(buttonSize)" :height="pxCheck(buttonSize)" />
      </slot>
    </view>
  </view>
</template>
<script lang="ts">
import { computed, watch } from 'vue';
import { createComponent } from '@/packages/utils/create';
import { pxCheck } from '@/packages/utils/pxCheck';
import { Minus, Plus } from '@nutui/icons-vue';
const { componentName, create } = createComponent('input-number');
export default create({
  components: { Minus, Plus },
  props: {
    modelValue: {
      type: [Number, String],
      default: 0
    },
    inputWidth: {
      type: [Number, String],
      default: ''
    },
    buttonSize: {
      type: [Number, String],
      default: ''
    },
    min: {
      type: [Number, String],
      default: 1
    },
    max: {
      type: [Number, String],
      default: 9999
    },
    step: {
      type: [Number, String],
      default: 1
    },
    decimalPlaces: {
      type: [Number, String],
      default: 0
    },
    disabled: {
      type: Boolean,
      default: false
    },
    readonly: {
      type: Boolean,
      default: false
    }
  },
  emits: ['update:modelValue', 'change', 'blur', 'focus', 'reduce', 'add', 'overlimit'],
  setup(props, { emit }) {
    const classes = computed(() => {
      const prefixCls = componentName;
      return {
        [prefixCls]: true,
        [`${prefixCls}--disabled`]: props.disabled
      };
    });
    const fixedDecimalPlaces = (v: string | number): string => {
      return Number(v).toFixed(Number(props.decimalPlaces));
    };
    const change = (event: Event) => {
      const input = event.target as HTMLInputElement;
      emit('update:modelValue', input.valueAsNumber, event);
    };
    const emitChange = (value: string | number, event: Event) => {
      let output_value: number | string = fixedDecimalPlaces(value);
      emit('update:modelValue', output_value, event);
      if (Number(props.modelValue) !== Number(output_value)) emit('change', output_value, event);
    };
    const addAllow = (value = Number(props.modelValue)): boolean => {
      return value < Number(props.max) && !props.disabled;
    };
    const reduceAllow = (value = Number(props.modelValue)): boolean => {
      return value > Number(props.min) && !props.disabled;
    };
    const reduce = (event: Event) => {
      emit('reduce', event);
      let output_value = Number(props.modelValue) - Number(props.step);
      if (reduceAllow() && output_value >= Number(props.min)) {
        emitChange(output_value, event);
      } else {
        emitChange(Number(props.min), event);
        emit('overlimit', event, 'reduce');
      }
    };
    const add = (event: Event) => {
      emit('add', event);
      let output_value = Number(props.modelValue) + Number(props.step);
      if (addAllow() && output_value <= Number(props.max)) {
        emitChange(output_value, event);
      } else {
        emitChange(Number(props.max), event);
        emit('overlimit', event, 'add');
      }
    };
    const focus = (event: Event) => {
      if (props.disabled) return;
      if (props.readonly) return;
      emit('focus', event);
    };
    const blur = (event: Event) => {
      if (props.disabled) return;
      if (props.readonly) return;
      const input = event.target as HTMLInputElement;
      let value = input.valueAsNumber;
      if (value < Number(props.min)) {
        value = Number(props.min);
      } else if (value > Number(props.max)) {
        value = Number(props.max);
      }
      emitChange(value, event);
      emit('blur', event);
    };
    const format = (val: string | number) => {
      let value = Number(val);
      if (value < Number(props.min)) {
        value = Number(props.min);
      } else if (value > Number(props.max)) {
        value = Number(props.max);
      }
      return value;
    };
    watch(
      () => [props.max, props.min],
      () => {
        if (Number(props.min) > Number(props.max)) {
          console.warn('[NutUI] <InputNumber>', 'props.max < props.min');
        }
        const value = format(props.modelValue);
        if (value !== Number(props.modelValue)) {
          emitChange(value, {} as Event);
        }
      }
    );

    return {
      classes,
      change,
      blur,
      focus,
      add,
      addAllow,
      reduce,
      reduceAllow,
      pxCheck
    };
  }
});
</script>
