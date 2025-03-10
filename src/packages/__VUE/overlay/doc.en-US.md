# Overlay

### Intro

Create a mask layer, which is usually used to prevent users from doing other operations

### Install

```js
import { createApp } from 'vue';
import { Overlay } from '@nutui/nutui';

const app = createApp();
app.use(Overlay);
```

### Basic Usage

Use `visible` to control the display/hiding of mask layers

:::demo

```vue
<template>
  <nut-button type="primary" @click="show = true">Show mask layer</nut-button>
  <nut-overlay v-model:visible="show"></nut-overlay>
</template>
<script setup>
import { ref } from 'vue';
const show = ref(false);
</script>
```

:::

### Mask style

It can be set through `overlay-style`

:::demo

```vue
<template>
  <nut-button type="primary" @click="show = true">Mask style</nut-button>
  <nut-overlay v-model:visible="show" :overlay-style="overlayStyle"></nut-overlay>
</template>
<script setup>
import { ref } from 'vue';
const show = ref(false);
const overlayStyle = ref({
  backgroundColor: 'rgba(0, 0, 0, .2)'
});
</script>
```

:::

### Set animation time

Set mask display/hide time through `duration`, time `s`

:::demo

```vue
<template>
  <nut-button type="primary" @click="show = true">Set animation time</nut-button>
  <nut-overlay v-model:visible="show" :duration="2.5"></nut-overlay>
</template>
<script setup>
import { ref } from 'vue';
const show = ref(false);
</script>
```

:::

### Lock Background Scroll

Use `lock scroll` to set whether the mask locks the background when displaying, and prohibit scrolling

:::demo

```vue
<template>
  <nut-button type="primary" @click="show = true">Lock Background Scroll</nut-button>
  <nut-overlay v-model:visible="show" :lock-scroll="true"></nut-overlay>
</template>
<script setup>
import { ref } from 'vue';
const show = ref(false);
</script>
```

:::

### Nested content

The default `slot` is supported, and any content can be nested

:::demo

```vue
<template>
  <nut-button type="success" @click="show = true">Nested content</nut-button>
  <nut-overlay v-model:visible="show">
    <div class="wrapper">
      <div class="content">Here is the text</div>
    </div>
  </nut-overlay>
</template>
<script setup>
import { ref } from 'vue';
const show = ref(false);
</script>
<style>
.wrapper {
  display: flex;
  height: 100%;
  align-items: center;
  justify-content: center;
}
.content {
  display: flex;
  width: 150px;
  height: 150px;
  background: #fff;
  border-radius: 8px;
  align-items: center;
  justify-content: center;
  color: red;
}
</style>
```

:::

### Click the mask not to close

Set `close on click override` to control whether the click mask is closed. If it is set to `true`, the `click` event will also become invalid

:::demo

```vue
<template>
  <nut-button type="primary" @click="show = true">Click the mask not to close</nut-button>
  <nut-overlay v-model:visible="show" :close-on-click-overlay="false">
    <div class="wrapper">
      <div class="content" @click.stop="show = false">close</div>
    </div>
  </nut-overlay>
</template>
<script setup>
import { ref } from 'vue';
const show = ref(false);
</script>
<style>
.wrapper {
  display: flex;
  height: 100%;
  align-items: center;
  justify-content: center;
}
.content {
  display: flex;
  width: 150px;
  height: 150px;
  background: #fff;
  border-radius: 8px;
  align-items: center;
  justify-content: center;
  color: red;
}
</style>
```

:::

## API

### Props

| Attribute              | Description                                                 | Type             | Default |
| ---------------------- | ----------------------------------------------------------- | ---------------- | ------- |
| v-model:visible        | Control the display/hide of masks                           | boolean          | `false` |
| z-index                | Custom Mask Level                                           | string \| number | `2000`  |
| duration               | Display/hide animation duration, in seconds                 | string \| number | `0.3`   |
| overlay-class          | Custom mask class name                                      | string           | -       |
| overlay-style          | Custom Mask Style                                           | CSSProperties    | -       |
| lock-scroll            | Whether the background is locked when the mask is displayed | boolean          | `true`  |
| close-on-click-overlay | Whether to close the mask when clicking.                    | boolean          | `true`  |

### Events

| Event | Description             | Arguments         |
| ----- | ----------------------- | ----------------- |
| click | Triggered when clicked. | event: MouseEvent |

### Slots

| Name    | Description                         |
| ------- | ----------------------------------- |
| default | Embedded Mask Content Customization |

## Theming

### CSS Variables

The component provides the following CSS variables, which can be used to customize styles. Please refer to [ConfigProvider component](#/en-US/component/configprovider).

| Name                   | Default Value        |
| ---------------------- | -------------------- |
| --nut-overlay-bg-color | _rgba(0, 0, 0, 0.7)_ |
