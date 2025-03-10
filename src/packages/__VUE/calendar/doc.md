# Calendar 日历

### 介绍

日历，可平铺/弹窗展示

### 安装

```javascript
import { createApp } from 'vue';
import { Calendar } from '@nutui/nutui';

const app = createApp();
app.use(Calendar);
```

### 基础用法

:::demo

```html
<template>
  <nut-cell
    :showIcon="true"
    title="选择单个日期"
    :desc="date ? `${date} ${dateWeek}` : '请选择'"
    @click="openSwitch('isVisible')"
  >
  </nut-cell>
  <nut-calendar
    v-model:visible="isVisible"
    :default-value="date"
    @close="closeSwitch('isVisible')"
    @choose="setChooseValue"
    :start-date="`2022-01-11`"
    :end-date="`2022-11-30`"
  >
  </nut-calendar>
</template>
<script lang="ts">
  import { reactive, toRefs } from 'vue';
  export default {
    setup() {
      const state = reactive({
        isVisible: false,
        date: '',
        dateWeek: ''
      });
      const openSwitch = (param) => {
        state[`${param}`] = true;
      };
      const closeSwitch = (param) => {
        state[`${param}`] = false;
      };
      const setChooseValue = (param) => {
        state.date = param[3];
        state.dateWeek = param[4];
      };
      return {
        ...toRefs(state),
        openSwitch,
        closeSwitch,
        setChooseValue
      };
    }
  };
</script>
```

:::

### 选择日期区间

:::demo

```html
<template>
  <nut-cell
    :showIcon="true"
    title="选择日期区间"
    :desc="date && date[0] ? `${date[0]}至${date[1]}` : '请选择'"
    @click="openSwitch('isVisible')"
  >
  </nut-cell>
  <nut-calendar
    v-model:visible="isVisible"
    :default-value="date"
    type="range"
    :start-date="`2019-12-22`"
    :end-date="`2021-01-08`"
    @close="closeSwitch('isVisible')"
    @choose="setChooseValue"
    @select="select"
  >
  </nut-calendar>
</template>
<script lang="ts">
  import { reactive, toRefs } from 'vue';
  export default {
    setup() {
      const state = reactive({
        date: ['2019-12-23', '2019-12-26'],
        isVisible: false
      });
      const openSwitch = (param) => {
        state[`${param}`] = true;
      };
      const closeSwitch = (param) => {
        state[`${param}`] = false;
      };
      const setChooseValue = (param) => {
        state.date = [...[param[0][3], param[1][3]]];
      };
      const select = (param: string) => {
        console.log(param);
      };
      return {
        ...toRefs(state),
        openSwitch,
        closeSwitch,
        setChooseValue,
        select
      };
    }
  };
</script>
<style>
  .nut-cell__value {
    flex: initial;
  }
</style>
```

:::

### 选择多个日期

:::demo

```html
<template>
  <nut-cell
    :show-icon="true"
    title="选择多个日期"
    :desc="date7 && date7.length ? `已选择${date7.length}个日期` : '请选择'"
    @click="openSwitch('isVisible7')"
  >
  </nut-cell>
  <nut-calendar
    v-model:visible="isVisible7"
    :default-value="date7"
    type="multiple"
    :start-date="`2022-01-01`"
    :end-date="`2022-09-10`"
    @close="closeSwitch('isVisible7')"
    @choose="setChooseValue7"
  >
  </nut-calendar>
</template>
<script lang="ts">
  import { reactive, toRefs } from 'vue';
  export default {
    setup() {
      const state = reactive({
        date7: [],
        isVisible7: false
      });
      const openSwitch = (param) => {
        state[`${param}`] = true;
      };
      const closeSwitch = (param) => {
        state[`${param}`] = false;
      };
      const setChooseValue7 = (chooseData: any) => {
        let dateArr = chooseData.map((item: any) => {
          return item[3];
        });
        state.date7 = [...dateArr];
      };
      const select = (param: string) => {
        console.log(param);
      };
      return {
        ...toRefs(state),
        openSwitch,
        closeSwitch,
        setChooseValue7,
        select
      };
    }
  };
</script>
```

:::

### 选择周

当设置为周选择时，会根据`first-day-of-week` 判断周的起始与结束日期。如`first-day-of-week`为 0 时，一周的起始日期为星期日。其他情况时，一周的起始日期为星期一。
:::demo

```html
<template>
  <nut-cell
    :showIcon="true"
    title="选择周"
    :desc="date && date[0] ? `${date[0]}至${date[1]}` : '请选择'"
    @click="openSwitch('isVisible')"
  >
  </nut-cell>
  <nut-calendar
    v-model:visible="isVisible"
    :default-value="date"
    type="week"
    :start-date="`2019-12-22`"
    :end-date="`2021-01-08`"
    @close="closeSwitch('isVisible')"
    @choose="setChooseValue"
    @select="select"
  >
  </nut-calendar>
</template>
<script lang="ts">
  import { reactive, toRefs } from 'vue';
  export default {
    setup() {
      const state = reactive({
        date: ['2019-12-23', '2019-12-26'],
        isVisible: false
      });
      const openSwitch = (param) => {
        state[`${param}`] = true;
      };
      const closeSwitch = (param) => {
        state[`${param}`] = false;
      };
      const setChooseValue = (param) => {
        let { weekDate } = param;
        state.date = [weekDate[0].date[3], weekDate[1].date[3]];
      };
      const select = (param: string) => {
        console.log(param);
      };
      return {
        ...toRefs(state),
        openSwitch,
        closeSwitch,
        setChooseValue,
        select
      };
    }
  };
</script>
<style>
  .nut-cell__value {
    flex: initial;
  }
</style>
```

:::

### 快捷选择-单选

:::demo

```html
<template>
  <nut-cell :showIcon="true" title="选择单个日期" :desc="date ? date : '请选择'" @click="openSwitch('isVisible')">
  </nut-cell>
  <nut-calendar
    v-model:visible="isVisible"
    @close="closeSwitch('isVisible')"
    @choose="setChooseValue"
    :default-value="date"
    :start-date="null"
    :end-date="null"
    :is-auto-back-fill="true"
  >
  </nut-calendar>
</template>
<script lang="ts">
  import { reactive, toRefs } from 'vue';
  export default {
    setup() {
      const state = reactive({
        date: '',
        isVisible: false
      });
      const openSwitch = (param) => {
        state[`${param}`] = true;
      };
      const closeSwitch = (param) => {
        state[`${param}`] = false;
      };
      const setChooseValue = (param) => {
        state.date = param[3];
      };
      return {
        ...toRefs(state),
        setChooseValue,
        openSwitch,
        closeSwitch
      };
    }
  };
</script>
```

:::

### 快捷选择-范围选择

:::demo

```html
<template>
  <nut-cell
    :showIcon="true"
    title="选择日期范围"
    :desc="date && date[0] ? `${date[0]}至${date[1]}` : '请选择'"
    @click="openSwitch('isVisible')"
  >
  </nut-cell>
  <nut-calendar
    v-model:visible="isVisible"
    :default-value="date"
    type="range"
    :start-date="`2022-01-01`"
    :end-date="`2022-12-31`"
    @close="closeSwitch('isVisible')"
    @choose="setChooseValue"
    :is-auto-back-fill="true"
  >
  </nut-calendar>
</template>
<script lang="ts">
  import { reactive, toRefs } from 'vue';
  export default {
    setup() {
      const state = reactive({
        date: ['2021-12-23', '2021-12-26'],
        isVisible: false
      });
      const openSwitch = (param) => {
        state[`${param}`] = true;
      };
      const closeSwitch = (param) => {
        state[`${param}`] = false;
      };
      const setChooseValue = (param) => {
        state.date = [...[param[0][3], param[1][3]]];
      };
      return {
        ...toRefs(state),
        setChooseValue,
        openSwitch,
        closeSwitch
      };
    }
  };
</script>
<style>
  .nut-cell__value {
    flex: initial;
  }
</style>
```

:::

### 自定义日历按钮

:::demo

```html
<template>
  <nut-cell
    :showIcon="true"
    title="自定义按钮"
    :desc="date && date[0] ? `${date[0]}至${date[1]}` : '请选择'"
    @click="openSwitch('isVisible')"
  >
  </nut-cell>
  <nut-calendar
    ref="calendarRef"
    v-model:visible="isVisible"
    :default-value="date"
    type="range"
    @close="closeSwitch('isVisible')"
    @choose="setChooseValue"
  >
    <template #btn>
      <div class="wrapper">
        <div class="d_div">
          <span class="d_btn" @click="goDate">去某个时间</span>
        </div>
        <div class="d_div">
          <span class="d_btn" @click="clickBtn">最近七天</span>
        </div>
        <div class="d_div">
          <span class="d_btn" @click="clickBtn1">当月</span>
        </div>
      </div>
    </template>
    <template #day="date">
      <span>{{ date.date.day }}</span>
    </template>
  </nut-calendar>
</template>

<script lang="ts">
  import { reactive, toRefs, ref } from 'vue';
  export default {
    setup() {
      const state = reactive({
        date: [],
        isVisible: false
      });
      const calendarRef = ref(null);
      const getNumTwoBit = function (n: number): string {
        n = Number(n);
        return (n > 9 ? '' : '0') + n;
      };
      const date2Str = function (date: Date, split?: string): string {
        split = split || '-';
        const y = date.getFullYear();
        const m = getNumTwoBit(date.getMonth() + 1);
        const d = getNumTwoBit(date.getDate());
        return [y, m, d].join(split);
      };
      const getDay = function (i: number): string {
        i = i || 0;
        let date = new Date();
        const diff = i * (1000 * 60 * 60 * 24);
        date = new Date(date.getTime() + diff);
        return date2Str(date);
      };
      const isLeapYear = function (y: number): boolean {
        return (y % 4 == 0 && y % 100 != 0) || y % 400 == 0;
      };
      const getMonthDays = function (year: string, month: string): number {
        if (/^0/.test(month)) {
          month = month.split('')[1];
        }
        return ([0, 31, isLeapYear(Number(year)) ? 29 : 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31] as number[])[
          month as any
        ];
      };
      const openSwitch = (param) => {
        state[`${param}`] = true;
      };
      const closeSwitch = (param) => {
        state[`${param}`] = false;
      };
      const setChooseValue = (param) => {
        state.date = param[3];
      };
      const clickBtn = () => {
        let date = [date2Str(new Date()), getDay(6)];
        state.date = date;
      };
      const clickBtn1 = () => {
        let date = new Date();
        let year = date.getFullYear();
        let month: any = date.getMonth() + 1;
        month = month < 10 ? '0' + month : month + '';
        let yearMonth = `${year}-${month}`;
        let currMonthDays = getMonthDays(year + '', month + '');
        state.date = [`${yearMonth}-01`, `${yearMonth}-${currMonthDays}`];
      };
      const goDate = () => {
        if (calendarRef.value) {
          var date1 = new Date();
          date1.setDate(date1.getDate() + 30);
          calendarRef.value.scrollToDate(date2Str(date1));
        }
      };
      return {
        ...toRefs(state),
        setChooseValue,
        openSwitch,
        closeSwitch,
        clickBtn,
        calendarRef,
        goDate,
        clickBtn1
      };
    }
  };
</script>
<style>
  .nut-cell__value {
    flex: initial;
  }
  .wrapper {
    display: flex;
    padding: 0 40px;
    justify-content: center;
  }
  .d_div {
    margin: 0px 5px;
  }
  .d_btn {
    background: #fa3f19;
    color: #fff;
    font-size: 12px;
    padding: 2px 8px;
    border-radius: 4px;
    display: inline-block;
  }
</style>
```

:::

### 自定义时间文案

:::demo

```html
<template>
  <nut-cell
    :showIcon="true"
    title="自定义时间文案"
    :desc="date && date[0] ? `${date[0]}至${date[1]}` : '请选择'"
    @click="openSwitch('isVisible')"
  >
  </nut-cell>
  <nut-calendar
    v-model:visible="isVisible"
    :default-value="date"
    type="range"
    @close="closeSwitch('isVisible')"
    @choose="setChooseValue"
    :start-date="`2022-01-01`"
    :end-date="`2022-12-31`"
    confirm-text="提交"
    start-text="入店"
    end-text="离店"
    title="日期选择"
  >
    <template #day="date">
      <span>{{ date.date.day <= 9 ? '0' + date.date.day : date.date.day }}</span>
    </template>
    <template #bottom-info="date">
      <span class="info">{{ date.date ? (date.date.day == 10 ? '十' : '') : '' }}</span>
    </template>
  </nut-calendar>
</template>
<script lang="ts">
  import { reactive, toRefs } from 'vue';
  export default {
    setup() {
      const state = reactive({
        date: [],
        isVisible: false
      });
      const openSwitch = (param) => {
        state[`${param}`] = true;
      };
      const closeSwitch = (param) => {
        state[`${param}`] = false;
      };
      const setChooseValue = (param) => {
        state.date = param[3];
      };
      return {
        ...toRefs(state),
        setChooseValue,
        openSwitch,
        closeSwitch
      };
    }
  };
</script>
<style>
  .nut-cell__value {
    flex: initial;
  }
</style>
```

:::

### 自定义底部区域 v4.1.6

:::demo

```html
<template>
  <nut-cell
    :show-icon="true"
    title="自定义底部区域"
    :desc="date ? `${date}` : '请选择'"
    @click="openSwitch('isVisible')"
  >
  </nut-cell>
  <nut-calendar
    v-model:visible="isVisible"
    :default-value="date"
    :poppable="true"
    :is-auto-back-fill="false"
    @close="closeSwitch('isVisible')"
    @select="setSelectalue"
  >
    <template #footer-info="dateInfo">
      <nut-button size="large" block round type="primary" :disabled="disabled" @click="clickBtn(dateInfo)"
        >{{ disabled?'偶数的日期不能选择':'奇数的日期可以选择' }}</nut-button
      >
    </template>
  </nut-calendar>
</template>

<script lang="ts">
  import { reactive, toRefs, ref } from 'vue';
  export default {
    setup() {
      const state = reactive({
        date: '2023-09-03',
        isVisible: false,
        disabled: false
      });
      const openSwitch = (param) => {
        state[`${param}`] = true;
      };
      const closeSwitch = (param) => {
        state[`${param}`] = false;
      };
      const setSelectalue = (param: any) => {
        state.disabled = param[2] % 2 === 0;
      };
      const clickBtn = (dateInfo: any) => {
        state.date = dateInfo.date[3];
        state.isVisible = false;
      };
      return {
        ...toRefs(state),
        setSelectalue,
        openSwitch,
        closeSwitch,
        clickBtn
      };
    }
  };
</script>
```

:::

### 自定义周起始日

:::demo

```html
<template>
  <nut-cell
    :showIcon="true"
    title="自定义周起始日"
    :desc="date ? `${date}` : '请选择'"
    @click="openSwitch('isVisible')"
  >
  </nut-cell>
  <nut-calendar
    v-model:visible="isVisible"
    :default-value="date"
    @close="closeSwitch('isVisible')"
    @choose="setChooseValue"
    :first-day-of-week="2"
  >
  </nut-calendar>
</template>
<script lang="ts">
  import { reactive, toRefs } from 'vue';
  export default {
    setup() {
      const state = reactive({
        isVisible: false,
        date: ''
      });
      const openSwitch = (param) => {
        state[`${param}`] = true;
      };
      const closeSwitch = (param) => {
        state[`${param}`] = false;
      };
      const setChooseValue = (param) => {
        state.date = param[3];
      };
      return {
        ...toRefs(state),
        openSwitch,
        closeSwitch,
        setChooseValue
      };
    }
  };
</script>
<style>
  .nut-cell__value {
    flex: initial;
  }
</style>
```

:::

### 平铺展示

:::demo

```html
<template>
  <div class="test-calendar-wrapper">
    <nut-calendar
      :poppable="false"
      :default-value="date"
      :is-auto-back-fill="true"
      @choose="setChooseValue"
      :start-date="`2020-02-01`"
      :end-date="`2020-12-30`"
    >
    </nut-calendar>
  </div>
</template>
<script lang="ts">
  import { reactive, toRefs } from 'vue';
  export default {
    setup() {
      const state = reactive({
        date: '2022-07-08'
      });
      const setChooseValue = (param) => {
        state.date = param[3];
      };
      return {
        ...toRefs(state),
        setChooseValue
      };
    }
  };
</script>
<style>
  .test-calendar-wrapper {
    display: flex;
    width: 100%;
    height: 560px;
    overflow: hidden;
  }
</style>
```

:::

## API

### Props

| 参数              | 说明                                                                           | 类型               | 默认值            |
| ----------------- | ------------------------------------------------------------------------------ | ------------------ | ----------------- |
| v-model:visible   | 是否可见                                                                       | boolean            | `false`           |
| type              | 类型，日期单择`one`，区间选择`range`,日期多选`multiple`,周选择`week`(`v4.0.1`) | string             | '`one`'           |
| poppable          | 是否弹窗状态展示                                                               | boolean            | `true`            |
| is-auto-back-fill | 自动回填                                                                       | boolean            | `false`           |
| title             | 显示标题                                                                       | string             | `日期选择`        |
| default-value     | 默认值，单个日期选择 `string`，其他为 `string[]`                               | string \| string[] | `null`            |
| start-date        | 开始日期                                                                       | string             | `今天`            |
| end-date          | 结束日期                                                                       | string             | `距离今天 365 天` |
| show-today        | 是否展示今天标记                                                               | boolean            | `true`            |
| start-text        | 范围选择，开始信息文案                                                         | string             | `开始`            |
| end-text          | 范围选择，结束信息文案                                                         | string             | `结束`            |
| confirm-text      | 底部确认按钮文案                                                               | string             | `确认`            |
| show-title        | 是否在展示日历标题                                                             | boolean            | `true`            |
| show-sub-title    | 是否展示日期标题                                                               | boolean            | `true`            |
| to-date-animation | 是否启动滚动动画                                                               | boolean            | `true`            |
| first-day-of-week | 设置周起始日                                                                   | 0-6                | `0`               |

### Events

| 事件名 | 说明                                                      | 回调参数                 |
| ------ | --------------------------------------------------------- | ------------------------ |
| choose | 选择之后或是点击确认按钮触发,日期数组（包含年月日和星期） | `(string \| string[])[]` |
| close  | 关闭时触发                                                | -                        |
| select | 点击/选择后触发                                           | `(string \| string[])[]` |

### Slots

| 名称                | 说明                                     |
| ------------------- | ---------------------------------------- |
| btn                 | 自定义日历标题下部，可用以添加自定义操作 |
| day                 | 日期信息                                 |
| top-info            | 日期顶部信息                             |
| bottom-info         | 日期底部信息                             |
| footer-info`v4.1.6` | 日历自定义底部，替代confirm按钮          |

### Methods

通过 [ref](https://vuejs.org/guide/essentials/template-refs.html) 可以获取到 `Calendar` 实例并调用实例方法。

| 方法名                | 说明                                  | 参数     |
| --------------------- | ------------------------------------- | -------- |
| scrollToDate          | 滚动到指定日期所在月,如：'2021-12-30' | `string` |
| initPosition `v4.0.1` | 初始化滚动位置                        | 无       |

## 主题定制

### 样式变量

组件提供了下列 CSS 变量，可用于自定义样式，使用方法请参考 [ConfigProvider 组件](#/zh-CN/component/configprovider)。

| 名称                                    | 默认值                     |
| --------------------------------------- | -------------------------- |
| --nut-calendar-primary-color            | _var(--nut-primary-color)_ |
| --nut-calendar-choose-color             | _var(--nut-primary-color)_ |
| --nut-calendar-choose-font-color        | _var(--nut-primary-color)_ |
| --nut-calendar-base-color               | _#333333_                  |
| --nut-calendar-disable-color            | _#d1d0d0_                  |
| --nut-calendar-base-font                | _var(--nut-font-size-3)_   |
| --nut-calendar-title-font               | _var(--nut-font-size-4)_   |
| --nut-calendar-title-font-weight        | _500_                      |
| --nut-calendar-sub-title-font           | _var(--nut-font-size-2)_   |
| --nut-calendar-text-font                | _var(--nut-font-size-1)_   |
| --nut-calendar-day-font                 | _16px_                     |
| --nut-calendar-day-active-border-radius | _0px_                      |
| --nut-calendar-day-font-weight          | _500_                      |
| --nut-calendar-day67-font-color         | _var(--nut-primary-color)_ |
| --nut-calendar-month-title-font-size    | _inherit_                  |
