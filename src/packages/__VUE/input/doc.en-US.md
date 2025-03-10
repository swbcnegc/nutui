# Input

### Intro

The user can enter content in the text box.

### Install

```js
import { createApp } from 'vue';
import { Input } from '@nutui/nutui';

const app = createApp();
app.use(Input);
```

### Basic Usage

The value of field is bound with `v-model`.

:::demo

```vue
<template>
  <nut-input v-model="val" placeholder="Text" />
</template>
<script setup>
import { ref } from 'vue';
const val = ref('');
</script>
```

:::

### Custom Type

Use `type` prop to custom different type fields.

:::demo

```vue
<template>
  <nut-input placeholder="Text" v-model="state.text" />
  <nut-input placeholder="Password" v-model="state.password" type="password" />
  <nut-input placeholder="Number" v-model="state.number" type="number" />
  <nut-input placeholder="Digit" v-model="state.digit" type="digit" />
</template>
<script setup>
import { reactive } from 'vue';
const state = reactive({
  text: '',
  password: '',
  number: '',
  digit: ''
});
</script>
```

:::

### Readonly And Disabled

Use `readonly` to set the input box to read-only status, and use `disabled` to set the input box to disabled status.

:::demo

```vue
<template>
  <nut-input placeholder="Readonly" v-model="state.readonly" readonly />
  <nut-input placeholder="Disabled" v-model="state.disabled" disabled />
</template>
<script setup>
import { reactive } from 'vue';
const state = reactive({
  readonly: '',
  disabled: ''
});
</script>
```

:::

### Show Clear Icon

Display the clear icon during input by setting 'clearable'. The clear button will be hidden after losing focus. You can set the clear button to continue to display after losing focus through 'showClearIcon'.

:::demo

```vue
<template>
  <nut-input v-model="state.clear" placeholder="Show Clear Icon" clearable clearSize="14" />
  <nut-input
    v-model="state.clear2"
    placeholder="Custom Clear Icon"
    clearable
    clearSize="14"
    show-word-limit
    max-length="50"
    :showClearIcon="true"
  >
    <template #clear>
      <Close width="12" height="12" @click="clearValue"></Close>
    </template>
  </nut-input>
</template>
<script setup>
import { reactive } from 'vue';
import { Close } from '@nutui/icons-vue';
const state = reactive({
  clear: '',
  clear2: ''
});
const clearValue = () => {
  state.clear2 = '';
};
</script>
```

:::

### Use With Form

Use in combination with `nut-form` and `nut-form-item`

:::demo

```vue
<template>
  <nut-form :model-value="state">
    <nut-form-item label="Text" label-align="center">
      <nut-input v-model="state.val1" placeholder="Text" :border="false" />
    </nut-form-item>
  </nut-form>
</template>
<script setup>
import { reactive } from 'vue';
const state = reactive({
  val1: ''
});
</script>
```

:::

### Format Value

Use `formatter` prop to format the input value.

:::demo

```vue
<template>
  <nut-input v-model="state.format1" placeholder="Format On Change" :formatter="formatter" format-trigger="onChange" />
  <nut-input v-model="state.format2" placeholder="Format On Blur" :formatter="formatter" format-trigger="onBlur" />
</template>
<script setup>
import { reactive } from 'vue';
const state = reactive({
  format1: '',
  format2: ''
});
const formatter = (value) => value.replace(/\d/g, '');
</script>
```

:::

### Show Word Limit

After setting the `maxlength` and `show-word-limit` attributes, word count will be displayed at the bottom.

:::demo

```vue
<template>
  <nut-input v-model="state.text" type="text" show-word-limit rows="2" max-length="50" placeholder="Message" />
</template>
<script setup>
import { ref } from 'vue';
const val = ref('');
</script>
```

:::

### No Border

:::demo

```vue
<template>
  <nut-input v-model="state.val1" :border="false" placeholder="No Border" />
  <nut-input v-model="state.val2" :border="false" placeholder="No Border" />
</template>
<script setup>
import { reactive } from 'vue';
const state = reactive({
  val1: '',
  val2: ''
});
</script>
```

:::

### Event Demo

:::demo

```vue
<template>
  <nut-input v-model="val" clearable placeholder="Event" @clear="clear" @click-input="clickInput" />
</template>
<script setup>
import { ref } from 'vue';
import { showToast } from '@nutui/nutui';
const val = ref('');
const clear = () => {
  showToast.text('clear');
};
const clickInput = () => {
  showToast.text('clickInput');
};
</script>
```

:::

### Slots Demo

:::demo

```vue
<template>
  <nut-input v-model="val" placeholder="Slots Demo" clearable>
    <template #left> <Ask></Ask> </template>
    <template #right> <nut-button type="primary" size="small">Verification Code</nut-button> </template>
  </nut-input>
</template>
<script setup>
import { reactive } from 'vue';
import { Ask } from '@nutui/icons-vue';
const val = ref('');
</script>
```

:::

## API

### Props

| Attribute | Description | Type | Default |
| --- | --- | --- | --- |
| v-model | Input value, two-way binding | string | - |
| type | Input type, support all native types and `number` `digit` type | string | `text` |
| placeholder | Placeholder when the input box is empty | string | - |
| input-align | Input align, eg `left`、`center`、`right` | string | `left` |
| required | Whether to show required mark | boolean | `false` |
| border | Whether to show inner borde | boolean | `true` |
| disabled | Whether to disable field | boolean | `false` |
| readonly | Whether to be readonly | boolean | `false` |
| autofocus | Whether to auto focus, unsupported in iOS | boolean | `false` |
| max-length | Max length of value | string ｜ number | - |
| clearable | Whether to be clearable | boolean | `false` |
| showClearIcon `4.0.2` | Whether to continue to display the clear button after losing focus, which will take effect when 'clearable' is set | boolean | `false` |
| clear-size | Clear Icon `font-size` | string | `14` |
| show-word-limit | Whether to show word limit, need to set the `max-length` prop | boolean | `false` |
| error | Whether to mark the input content in red | boolean | `false` |
| formatter | Input value formatter | `(val: string) => string` | - |
| format-trigger | When to format value, eg `onChange`、`onBlur` | string | `onChange` |
| confirm-type | The text of the button in the lower right corner of the keyboard, only valid when `type='text'`, eg `send`, `search`, `next`, ` go`, `done` | string | `done` |

### Events

| Event | Description | Arguments |
| --- | --- | --- |
| update:model-value | Emitted when input value changed | `val` |
| focus | Emitted when input is focused | `event` |
| blur | Emitted when input is blurred | `event` |
| clear | Emitted when the clear icon is clicked | `event` |
| click | Emitted when component is clicked | `event` |
| click-input | Emitted when the input is clicked | `event` |

### Slots

| Name | Description |
| --- | --- |
| clear | Customize the end of the input box to clear the button |
| left `4.0.1` | Customize the slot content on the left side of the input box |
| right `4.0.1` | Customize the slot content on the right side of the input box |

### Ref

| Name | Description |
| --- | --- |
| focus `4.0.6` | Focus |
| blur `4.0.6` | Blur |
| select `4.0.6` | Selct |

## Theming

### CSS Variables

The component provides the following CSS variables, which can be used to customize styles. Please refer to [ConfigProvider component](#/en-US/component/configprovider).

| Name | Default Value |
| --- | --- |
| --nut-input-border-bottom | _#eaf0fb_ |
| --nut-input-disabled-color | _#c8c9cc_ |
| --nut-input-required-color | _var(--nut-required-color)_ |
| --nut-input-font-size | _var(--nut-font-size-2)_ |
