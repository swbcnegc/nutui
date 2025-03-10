# TabBar 标签栏

### 介绍

底部导航常用场景

### 安装

```js
import { createApp } from 'vue';
import { Tabbar, TabbarItem } from '@nutui/nutui';

const app = createApp();
app.use(Tabbar);
app.use(TabbarItem);
```

### 基础用法

:::demo

```vue
<template>
  <nut-tabbar @tab-switch="tabSwitch">
    <nut-tabbar-item tab-title="标签">
      <template #icon>
        <Home></Home>
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签">
      <template #icon>
        <Category></Category>
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签">
      <template #icon>
        <Find></Find>
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签">
      <template #icon>
        <Cart></Cart>
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签">
      <template #icon>
        <My></My>
      </template>
    </nut-tabbar-item>
  </nut-tabbar>
</template>
<script setup>
import { Home, Category, Find, Cart, My } from '@nutui/icons-vue';
const tabSwitch = (item, index) => {
  console.log(item, index);
};
</script>
```

:::

### 通过名称匹配

:::demo

```vue
<template>
  <nut-tabbar v-model="activeName" @tab-switch="tabSwitch">
    <nut-tabbar-item v-for="(item, index) in List" :name="item.name" :tab-title="item.title" :icon="item.icon">
    </nut-tabbar-item>
  </nut-tabbar>
</template>
<script setup>
import { h, ref } from 'vue';
import { Home, Category, Find, Cart, My } from '@nutui/icons-vue';
const activeName = ref('category');
const List = [
  {
    title: '标签',
    icon: h(Home),
    name: 'home'
  },
  {
    title: '标签',
    icon: h(Category),
    name: 'category'
  },
  {
    title: '标签',
    icon: h(Find),
    name: 'find'
  },
  {
    title: '标签',
    icon: h(Cart),
    name: 'cart'
  },
  {
    title: '标签',
    icon: h(My),
    name: 'my'
  }
];
const tabSwitch = (item, index) => {
  console.log(item, index);
};
</script>
```

:::

### 自定义图标

通过 icon 插槽自定义图标，可以通过 slot-scope 判断标签是否选中

:::demo

```vue
<template>
  <nut-tabbar @tab-switch="tabSwitch">
    <nut-tabbar-item tab-title="标签">
      <template #icon="props">
        <img :src="props.active ? icon.active : icon.unactive" alt="" />
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签">
      <template #icon="props">
        <img :src="props.active ? icon.active : icon.unactive" alt="" />
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签">
      <template #icon="props">
        <img :src="props.active ? icon.active : icon.unactive" alt="" />
      </template>
    </nut-tabbar-item>
  </nut-tabbar>
</template>
<script setup>
const tabSwitch = (item, index) => {
  console.log(item, index);
};
const icon = {
  active: 'https://img11.360buyimg.com/imagetools/jfs/t1/70423/4/20553/3652/62e74629E23ba550e/aeeed0e3b9f43ae6.png',
  unactive: 'https://img13.360buyimg.com/imagetools/jfs/t1/23319/19/18329/3084/62e7c346E957c54ef/6c3e8a49e52b76f2.png'
};
</script>
```

:::

### 自定义选中

:::demo

```vue
<template>
  <nut-tabbar v-model="active">
    <nut-tabbar-item v-for="(item, index) in List" :tab-title="item.title" :icon="item.icon"> </nut-tabbar-item>
  </nut-tabbar>
</template>
<script setup>
import { h, ref } from 'vue';
import { Home, Category, Find, Cart, My } from '@nutui/icons-vue';
const active = ref(2);
const List = [
  {
    title: '标签',
    icon: h(Home),
    name: 'home'
  },
  {
    title: '标签',
    icon: h(Category),
    name: 'category'
  },
  {
    title: '标签',
    icon: h(Find),
    name: 'find'
  },
  {
    title: '标签',
    icon: h(Cart),
    name: 'cart'
  },
  {
    title: '标签',
    icon: h(My),
    name: 'my'
  }
];
const tabSwitch = (item, index) => {
  console.log(item, index);
};
</script>
```

:::

### 徽标提示

设置 dot 属性后，会在图标右上角展示一个小红点；设置 value 属性后，会在图标右上角展示相应的徽标。

内部使用了 bardge 徽标组件，可将其属性直接作用于 TabbarItem props。
:::demo

```vue
<template>
  <nut-tabbar>
    <nut-tabbar-item tab-title="标签" :value="11">
      <template #icon>
        <Home></Home>
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签" :dot="true">
      <template #icon>
        <Home></Home>
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签" :value="110">
      <template #icon>
        <Home></Home>
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签">
      <template #icon>
        <Home></Home>
      </template>
    </nut-tabbar-item>
  </nut-tabbar>
</template>
<script setup>
import { Home, Category, Find, Cart, My } from '@nutui/icons-vue';
</script>
```

:::

### 自定义颜色

:::demo

```vue
<template>
  <nut-tabbar unactive-color="#7d7e80" active-color="#1989fa">
    <nut-tabbar-item v-for="(item, index) in List" :tab-title="item.title" :icon="item.icon"> </nut-tabbar-item>
  </nut-tabbar>
</template>
<script setup>
import { h, ref } from 'vue';
import { Home, Category, Find, Cart, My } from '@nutui/icons-vue';
const List = [
  {
    title: '标签',
    icon: h(Home),
    name: 'home'
  },
  {
    title: '标签',
    icon: h(Category),
    name: 'category'
  },
  {
    title: '标签',
    icon: h(Find),
    name: 'find'
  },
  {
    title: '标签',
    icon: h(Cart),
    name: 'cart'
  },
  {
    title: '标签',
    icon: h(My),
    name: 'my'
  }
];
</script>
```

:::

### 自定义数量

:::demo

```vue
<template>
  <nut-tabbar unactive-color="#7d7e80" active-color="#1989fa">
    <nut-tabbar-item tab-title="标签" :value="11">
      <template #icon>
        <Home></Home>
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签" :dot="true">
      <template #icon>
        <Home></Home>
      </template>
    </nut-tabbar-item>
    <nut-tabbar-item tab-title="标签" :value="110">
      <template #icon>
        <Home></Home>
      </template>
    </nut-tabbar-item>
  </nut-tabbar>
</template>
<script setup>
import { Home } from '@nutui/icons-vue';
</script>
```

:::

### 固定底部，可自由跳转

:::demo

```vue
<template>
  <nut-tabbar bottom safe-area-inset-bottom placeholder>
    <nut-tabbar-item v-for="(item, index) in List" :tab-title="item.title" :icon="item.icon"> </nut-tabbar-item>
  </nut-tabbar>
</template>
<script setup>
import { h } from 'vue';
import { Home, Category, Find, Cart, My } from '@nutui/icons-vue';
const List = [
  {
    title: '标签',
    icon: h(Home)
  },
  {
    title: '标签',
    icon: h(Category)
  },
  {
    title: '标签',
    icon: h(Find)
  },
  {
    title: '标签',
    icon: h(Cart)
  },
  {
    title: '标签',
    icon: h(My)
  }
];
</script>
```

:::

## API

### Tabbar Props

| 参数                   | 说明                                               | 类型    | 默认值    |
| ---------------------- | -------------------------------------------------- | ------- | --------- |
| v-model                | 选中标签的索引值或者名称                           | number  | `0`       |
| bottom                 | 是否固定在页面底部                                 | boolean | `false`   |
| unactive-color         | icon 未激活的颜色                                  | string  | `#7d7e80` |
| active-color           | icon 激活的颜色                                    | string  | `#1989fa` |
| safe-area-inset-bottom | 是否开启 iphone 系列全面屏底部安全区适配           | boolean | `false`   |
| placeholder            | 固定在底部时，是否在标签位置生成一个等高的占位元素 | boolean | `false`   |

### TabbarItem Props

| 参数      | 说明                                                                                     | 类型             | 默认值             |
| --------- | ---------------------------------------------------------------------------------------- | ---------------- | ------------------ |
| tab-title | 标签页的标题                                                                             | string           | --                 |
| name      | 标签名称，作为匹配的标识符                                                               | string           | `当前标签的索引值` |
| icon      | 标签页显示的[图标](#/zh-CN/component/icon)                                               | Component        | --                 |
| href      | 标签页的跳转链接；如果同时存在 `to`，优先级高于 `to`                                     | string           | --                 |
| to        | 标签页的路由对象，等于 vue-router 的 [to 属性](https://router.vuejs.org/zh/api/#to) 属性 | string \| object | --                 |

### TabbarItem Slots

| 事件名 | 说明       | 参数            |
| ------ | ---------- | --------------- |
| icon   | 自定义图标 | active: boolean |

### Tabbar Events

| 事件名     | 说明               | 回调参数           |
| ---------- | ------------------ | ------------------ |
| tab-switch | 切换页签时触发事件 | 点击的数据和索引值 |

## 主题定制

### 样式变量

组件提供了下列 CSS 变量，可用于自定义样式，使用方法请参考 [ConfigProvider 组件](#/zh-CN/component/configprovider)。

| 名称                               | 默认值                     |
| ---------------------------------- | -------------------------- |
| --nut-tabbar-active-color          | _var(--nut-primary-color)_ |
| --nut-tabbar-unactive-color        | _var(--nut-primary-color)_ |
| --nut-tabbar-border-top            | _1px solid #eee_           |
| --nut-tabbar-border-bottom         | _1px solid #eee_           |
| --nut-tabbar-box-shadow            | _none_                     |
| --nut-tabbar-item-text-font-size   | _var(--nut-font-size-0)_   |
| --nut-tabbar-item-text-line-height | _initial_                  |
| --nut-tabbar-height                | _50px_                     |
| --nut-tabbar-word-margin-top       | _auto_                     |
