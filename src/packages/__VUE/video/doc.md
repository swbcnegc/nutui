# Video 视频播放器

### 介绍

原生 video 实现的视频播放器

### 安装

```javascript
import { createApp } from 'vue';
import { Video, Button } from '@nutui/nutui';

const app = createApp();
app.use(Video);
```

### 基础用法

:::demo

```html
<template>
  <nut-video :source="source" :options="options" @play="play" @pause="pause" @playend="playend"> </nut-video>
</template>
<script lang="ts">
  import { toRefs, reactive } from 'vue';
  export default {
    setup() {
      const state = reactive({
        source: {
          src: 'https://storage.jd.com/about/big-final.mp4?Expires=3730193075&AccessKey=3LoYX1dQWa6ZXzQl&Signature=ViMFjz%2BOkBxS%2FY1rjtUVqbopbJI%3D',
          type: 'video/mp4'
        },
        options: {
          controls: true
        }
      });
      const play = (elm: any) => console.log('play', elm);
      const pause = (elm: any) => console.log('pause', elm);
      const playend = (elm: any) => console.log('playend', elm);

      return { ...toRefs(state), play, pause, playend };
    }
  };
</script>
```

:::

### 自动播放

autoplay 属性设置视频自动播放

:::demo

```html
<template>
  <nut-video :source="source" :options="options"></nut-video>
</template>
<script lang="ts">
  import { toRefs, reactive } from 'vue';
  export default {
    setup() {
      const state = reactive({
        source: {
          src: 'https://storage.jd.com/about/big-final.mp4?Expires=3730193075&AccessKey=3LoYX1dQWa6ZXzQl&Signature=ViMFjz%2BOkBxS%2FY1rjtUVqbopbJI%3D',
          type: 'video/mp4'
        },
        options: {
          autoplay: true,
          muted: true,
          controls: true
        }
      });

      return { ...toRefs(state) };
    }
  };
</script>
```

:::

### 初始化静音

muted 属性设置视频初始化静音

:::demo

```html
<template>
  <nut-video :source="source" :options="options"></nut-video>
</template>
<script lang="ts">
  import { toRefs, reactive } from 'vue';
  export default {
    setup() {
      const state = reactive({
        source: {
          src: 'https://storage.jd.com/about/big-final.mp4?Expires=3730193075&AccessKey=3LoYX1dQWa6ZXzQl&Signature=ViMFjz%2BOkBxS%2FY1rjtUVqbopbJI%3D',
          type: 'video/mp4'
        },
        options: {
          muted: true,
          controls: true
        }
      });

      return { ...toRefs(state) };
    }
  };
</script>
```

:::

### 视频封面海报设置

poster 属性设置视频海报

:::demo

```html
<template>
  <nut-video :source="source" :options="options"></nut-video>
</template>
<script lang="ts">
  import { toRefs, reactive } from 'vue';
  export default {
    setup() {
      const state = reactive({
        source: {
          src: 'https://storage.jd.com/about/big-final.mp4?Expires=3730193075&AccessKey=3LoYX1dQWa6ZXzQl&Signature=ViMFjz%2BOkBxS%2FY1rjtUVqbopbJI%3D',
          type: 'video/mp4'
        },
        options: {
          controls: true,
          poster:
            'https://img12.360buyimg.com/ling/s345x208_jfs/t1/168105/33/8417/54825/603df06dEfcddc4cb/21f9f5d0a1b3dad4.jpg.webp'
        }
      });

      return { ...toRefs(state) };
    }
  };
</script>
```

:::

### 行内播放

playsinline 属性设置移动端视频行内播放，阻止新打开页面播放（兼容 ios，兼容部分安卓机）

:::demo

```html
<template>
  <nut-video :source="source" :options="options"></nut-video>
</template>
<script lang="ts">
  import { toRefs, reactive } from 'vue';
  export default {
    setup() {
      const state = reactive({
        source: {
          src: 'https://storage.jd.com/about/big-final.mp4?Expires=3730193075&AccessKey=3LoYX1dQWa6ZXzQl&Signature=ViMFjz%2BOkBxS%2FY1rjtUVqbopbJI%3D',
          type: 'video/mp4'
        },
        options: {
          playsinline: true,
          controls: true
        }
      });

      return { ...toRefs(state) };
    }
  };
</script>
```

:::

### 视频背景图

当设置视频为背景图时需要将 muted 静音、 disabled 禁止操作、loop 循环播放、autoplay 自动播放设置为 true，移动端需要设置 playsinline 行内展示

:::demo

```html
<template>
  <nut-video :source="source" :options="options"></nut-video>
</template>
<script lang="ts">
  import { toRefs, reactive } from 'vue';
  export default {
    setup() {
      const state = reactive({
        source: {
          src: 'https://storage.jd.com/about/big-final.mp4?Expires=3730193075&AccessKey=3LoYX1dQWa6ZXzQl&Signature=ViMFjz%2BOkBxS%2FY1rjtUVqbopbJI%3D',
          type: 'video/mp4'
        },
        options: {
          controls: false,
          autoplay: true,
          muted: true,
          disabled: true,
          playsinline: true,
          loop: true
        }
      });

      return { ...toRefs(state) };
    }
  };
</script>
```

:::

### 视频切换

当视频地址发生变化时，重置视频

:::demo

```html
<template>
  <nut-video :source="source1" :options="options"></nut-video>
  <nut-button type="primary" @click="changeVideo">切换视频</nut-button>
</template>
<script lang="ts">
  import { toRefs, reactive } from 'vue';
  export default {
    setup() {
      const state = reactive({
        source1: {
          src: 'https://storage.360buyimg.com/nutui/video/video_NutUI.mp4',
          type: 'video/mp4'
        },
        options: {
          controls: true
        }
      });
      const changeVideo = () => {
        state.source1.src = 'https://vjs.zencdn.net/v/oceans.mp4';
      };

      return { ...toRefs(state), changeVideo };
    }
  };
</script>
```

:::

### Ref 控制播放，暂停，结束，静音，取消静音 v4.1.6

:::demo

```html
<template>
  <nut-video ref="videoRef" :source="source" :options="options" @play="play" @pause="pause" @playend="playend">
  </nut-video>
  <nut-button type="success" class="m-b" @click="videoRef.play();">播放</nut-button>
  <nut-button type="warning" class="m-b" @click="videoRef.pause();">暂停</nut-button>
  <nut-button type="danger" class="m-b" @click="videoRef.stop();">结束</nut-button>
  <nut-button type="success" class="m-b" @click="videoRef.muted();">静音</nut-button>
  <nut-button type="danger" class="m-b" @click="videoRef.unmuted();">取消静音</nut-button>
</template>
<script lang="ts">
  import { toRefs, reactive, ref, Ref } from 'vue';
  export default {
    setup() {
      const state = reactive({
        source: {
          src: 'https://storage.jd.com/about/big-final.mp4?Expires=3730193075&AccessKey=3LoYX1dQWa6ZXzQl&Signature=ViMFjz%2BOkBxS%2FY1rjtUVqbopbJI%3D',
          type: 'video/mp4'
        },
        options: {
          controls: true
        }
      });
      const videoRef = ref(null) as Ref;
      const play = (elm: any) => console.log('play', elm);
      const pause = (elm: any) => console.log('pause', elm);
      const playend = (elm: any) => console.log('playend', elm);

      return { ...toRefs(state), play, pause, playend, videoRef };
    }
  };
</script>
```

:::

## API

### Props

| 参数    | 说明               | 类型         | 默认值 |
| ------- | ------------------ | ------------ | ------ |
| source  | 视频地址和类型设置 | VideoSource  | {}     |
| options | 控制视频播放属性   | VideoOptions | {}     |

### VideoSource 数据结构

| 键名 | 说明     | 类型    |
| ---- | -------- | ------- |
| src  | 视频链接 | boolean |
| type | 视频类型 | string  |

### VideoOptions 数据结构

| 键名        | 说明                                       | 类型    | 默认值  |
| ----------- | ------------------------------------------ | ------- | ------- |
| autoplay    | 是否自动播放                               | boolean | `false` |
| poster      | 海报设置                                   | string  | -       |
| loop        | 是否循环播放                               | boolean | `false` |
| controls    | 是否展示操作栏                             | boolean | `true`  |
| muted       | 是否静音                                   | boolean | `false` |
| volume      | 音量控制                                   | number  | `0.5`   |
| disabled    | 禁用操作（如循环播放的背景图，禁止操作）   | boolean | `false` |
| playsinline | 是否设置为行内播放元素（解决安卓兼容问题） | boolean | `false` |

### Events

| 事件名  | 说明                                               | 回调参数                      |
| ------- | -------------------------------------------------- | ----------------------------- |
| play    | 播放                                               | videoElm                      |
| pause   | 暂停                                               | videoElm                      |
| playend | 播放完成回调                                       | videoElm                      |
| time    | 播放时触发(current 为当前播放时间，total 为总时间) | (current:string,total:string) |

### Ref v4.1.6

| 事件名  | 说明     |
| ------- | -------- |
| play    | 播放     |
| pause   | 暂停     |
| stop    | 结束     |
| muted   | 静音     |
| unmuted | 取消静音 |
