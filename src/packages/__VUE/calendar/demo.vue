<template>
  <div class="demo">
    <h2>{{ translate('title') }}</h2>
    <div>
      <nut-cell
        :show-icon="true"
        :title="translate('single')"
        :desc="date ? `${date} ${dateWeek}` : translate('please')"
        @click="openSwitch('isVisible')"
      >
      </nut-cell>
      <nut-calendar
        v-model:visible="isVisible"
        :default-value="date"
        :start-date="`2022-01-11`"
        :end-date="`2022-11-30`"
        @close="closeSwitch('isVisible')"
        @choose="setChooseValue"
      >
      </nut-calendar>
    </div>

    <div>
      <nut-cell
        :show-icon="true"
        :title="translate('range')"
        :desc="date1.length ? `${date1[0]}${translate('conjunction')}${date1[1]}` : translate('please')"
        @click="openSwitch('isVisible1')"
      >
      </nut-cell>
      <nut-calendar
        ref="calendarRef"
        v-model:visible="isVisible1"
        :default-value="date1"
        type="range"
        :start-date="`2019-12-22`"
        :end-date="`2021-01-08`"
        @close="closeSwitch('isVisible1')"
        @choose="setChooseValue1"
        @select="select"
      >
      </nut-calendar>
    </div>
    <div>
      <nut-cell
        :show-icon="true"
        :title="translate('multiple')"
        :desc="date7 && date7.length ? `${translate('selected')}${date7.length}` : translate('please')"
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
    </div>
    <div>
      <nut-cell
        :show-icon="true"
        :title="translate('week')"
        :desc="date9.length ? `${date9[0]}${translate('conjunction')}${date9[1]}` : translate('please')"
        @click="openSwitch('isVisible9')"
      >
      </nut-cell>
      <nut-calendar
        v-model:visible="isVisible9"
        :default-value="date9"
        type="week"
        :start-date="`2019-12-22`"
        :end-date="`2021-01-08`"
        :first-day-of-week="1"
        @close="closeSwitch('isVisible9')"
        @choose="setChooseValue9"
        @select="select"
      >
      </nut-calendar>
    </div>
    <h2>{{ translate('title1') }}</h2>
    <div>
      <nut-cell
        :show-icon="true"
        :title="translate('single')"
        :desc="date3 ? date3 : translate('please')"
        @click="openSwitch('isVisible3')"
      >
      </nut-cell>
      <nut-calendar
        v-model:visible="isVisible3"
        :default-value="date3"
        :start-date="null"
        :end-date="null"
        :is-auto-back-fill="true"
        @close="closeSwitch('isVisible3')"
        @choose="setChooseValue3"
      >
      </nut-calendar>
    </div>
    <div>
      <nut-cell
        :show-icon="true"
        :title="translate('range')"
        :desc="date4.length ? `${date4[0]}${translate('conjunction')}${date4[1]}` : translate('please')"
        @click="openSwitch('isVisible4')"
      >
      </nut-cell>
      <nut-calendar
        v-model:visible="isVisible4"
        :default-value="date4"
        type="range"
        :start-date="`2022-01-01`"
        :end-date="`2022-12-31`"
        :is-auto-back-fill="true"
        @close="closeSwitch('isVisible4')"
        @choose="setChooseValue4"
      >
      </nut-calendar>
    </div>
    <h2>{{ translate('title2') }}</h2>
    <div>
      <nut-cell
        :show-icon="true"
        :title="translate('custom_btn')"
        :desc="date5 && date5[0] ? `${date5[0]}${translate('conjunction')}${date5[1]}` : translate('please')"
        @click="openSwitch('isVisible5')"
      >
      </nut-cell>
      <nut-calendar
        ref="calendarRef"
        v-model:visible="isVisible5"
        :default-value="date5"
        type="range"
        @close="closeSwitch('isVisible5')"
        @choose="setChooseValue5"
      >
        <template #btn>
          <div class="wrapper">
            <div class="d_div">
              <span class="d_btn" @click="goDate">{{ translate('goDate') }}</span>
            </div>
            <div class="d_div">
              <span class="d_btn" @click="clickBtn">{{ translate('seven') }}</span>
            </div>
            <div class="d_div">
              <span class="d_btn" @click="clickBtn1">{{ translate('current') }}</span>
            </div>
          </div>
        </template>
        <template #day="date">
          <span>{{ date.date.day }}</span>
        </template>
      </nut-calendar>
    </div>
    <div>
      <nut-cell
        :show-icon="true"
        :title="translate('timeText')"
        :desc="date6 && date6[0] ? `${date6[0]}${translate('conjunction')}${date6[1]}` : translate('please')"
        @click="openSwitch('isVisible6')"
      >
      </nut-cell>
      <nut-calendar
        v-model:visible="isVisible6"
        :default-value="date6"
        type="range"
        :start-date="`2022-01-01`"
        :end-date="`2022-12-31`"
        confirm-text="submit"
        :start-text="translate('enter')"
        :end-text="translate('leave')"
        :title="translate('please')"
        @close="closeSwitch('isVisible6')"
        @choose="setChooseValue6"
      >
        <template #day="date">
          <span>{{ renderDate(date) }}</span>
        </template>
        <template #bottom-info="date">
          <span class="info">{{ date.date ? (date.date.day == 10 ? '十' : '') : '' }}</span>
        </template>
      </nut-calendar>
    </div>
    <div>
      <nut-cell
        :show-icon="true"
        :title="translate('custom_footer')"
        :desc="date10 ? `${date10}` : translate('please')"
        @click="openSwitch('isVisible10')"
      >
      </nut-cell>
      <nut-calendar
        v-model:visible="isVisible10"
        :default-value="date10"
        :poppable="true"
        :is-auto-back-fill="false"
        @close="closeSwitch('isVisible10')"
        @select="setSelectalue10"
      >
        <template #footer-info="dateInfo">
          <nut-button size="large" block round type="primary" :disabled="disabled10" @click="clickBtn10(dateInfo)">{{
            disabled10 ? translate('custom_footer_text1') : translate('custom_footer_text2')
          }}</nut-button>
        </template>
      </nut-calendar>
    </div>
    <h2>{{ translate('title3') }}</h2>
    <div>
      <nut-cell
        :show-icon="true"
        :title="translate('single')"
        :desc="date8 ? `${date8}` : translate('please')"
        @click="openSwitch('isVisible8')"
      >
      </nut-cell>
      <nut-calendar
        v-model:visible="isVisible8"
        :default-value="date8"
        :first-day-of-week="2"
        @close="closeSwitch('isVisible8')"
        @choose="setChooseValue8"
      >
      </nut-calendar>
    </div>
    <h2>{{ translate('title4') }}</h2>
    <div class="test-calendar-wrapper">
      <nut-calendar
        :poppable="false"
        :default-value="date2"
        :is-auto-back-fill="true"
        :start-date="`2020-02-01`"
        :end-date="`2020-12-30`"
        @choose="setChooseValue2"
      >
      </nut-calendar>
    </div>
  </div>
</template>

<script lang="ts">
import { reactive, toRefs, ref, defineComponent } from 'vue';
import { createComponent } from '@/packages/utils/create';
import Utils from '@/packages/utils/date';
import { useTranslate } from '@/sites/assets/util/useTranslate';
import { CalendarRef, Day } from '../calendaritem/type';

const { translate } = createComponent('calendar');
const initTranslate = () =>
  useTranslate({
    'zh-CN': {
      title: '基础用法',
      title1: '快捷选择',
      title2: '自定义日历',
      title3: '自定义周起始日',
      title4: '平铺展示',

      please: '请选择',
      single: '选择单个日期',
      range: '选择日期区间',
      multiple: '选择多个日期',
      week: '选择周',

      conjunction: '至',
      custom_btn: '自定义按钮',
      timeText: '自定义时间文案',
      custom_footer: '自定义底部',
      custom_footer_text1: '偶数的日期不能选择',
      custom_footer_text2: '奇数的日期可以选择',

      goDate: '去某个月',
      seven: '最近七天',
      current: '当月',
      enter: '入店',
      leave: '离店',
      mid: '中旬',
      selected: '已选择：'
    },
    'en-US': {
      title: 'Basic Usage',
      title1: 'Quick Select',
      title2: 'Custom Calendar',
      title3: 'Custom First Day Of Week',
      title4: 'Tiled Display',

      please: 'Please Select Date',
      single: 'Select Single Date',
      range: 'Select Date Range',
      multiple: 'Select Multiple Date',
      week: 'Select Week',

      conjunction: '-',
      custom_btn: 'Custom Button',
      timeText: 'Custom Date Text',
      custom_footer: 'Custom Footer',
      custom_footer_text1: 'Even dates cannot be selected',
      custom_footer_text2: 'Odd dates can be selected',

      goDate: 'Go Date',
      seven: 'Last Seven Days',
      current: 'This Month',
      enter: 'enter',
      leave: 'leave',
      mid: 'mid',
      selected: 'selected:'
    }
  });
export default defineComponent({
  props: {},
  setup() {
    initTranslate();
    const calendarRef = ref<null | CalendarRef>(null);
    const state = reactive({
      isVisible: false,
      date: '2022-02-01',
      dateWeek: '',
      date1: ['2020-01-23', '2020-01-26'],
      date2: '2020-07-08',
      date3: '',
      date4: ['2021-12-23', '2021-12-26'],
      date5: ['2021-12-23', '2021-12-26'],
      date6: [] as string[],
      date7: [] as string[],
      date8: '',
      date9: [] as string[],
      date10: '2023-09-03',
      isVisible1: false,
      isVisible2: false,
      isVisible3: false,
      isVisible4: false,
      isVisible5: false,
      isVisible6: false,
      isVisible7: false,
      isVisible8: false,
      isVisible9: false,
      isVisible10: false,
      disabled10: false
    });
    const openSwitch = (param: string) => {
      (state as any)[`${param}`] = true;
    };

    const closeSwitch = (param: string) => {
      (state as any)[`${param}`] = false;
    };

    const setChooseValue = (param: string) => {
      state.date = param[3];
      state.dateWeek = param[4];
    };

    const select = (param: string) => {
      console.log(param);
    };
    const setChooseValue1 = (chooseData: any) => {
      let dateArr = chooseData.map((item: any) => {
        return item[3];
      });
      state.date1 = [...dateArr];
    };
    const setChooseValue2 = (param: string) => {
      state.date2 = param[3];
    };

    const setChooseValue3 = (param: string) => {
      state.date3 = param[3];
    };
    const setChooseValue4 = (param: string) => {
      state.date4 = [...[param[0][3], param[1][3]]];
    };

    const setChooseValue5 = (param: string) => {
      state.date5 = [...[param[0][3], param[1][3]]];
    };
    const setChooseValue6 = (param: string) => {
      state.date6 = [...[param[0][3], param[1][3]]];
    };
    const setChooseValue7 = (chooseData: any) => {
      let dateArr = chooseData.map((item: any) => {
        return item[3];
      });
      state.date7 = [...dateArr];
    };
    const setChooseValue8 = (param: string) => {
      state.date8 = param[3];
    };
    const setChooseValue9 = (param: any) => {
      let { weekDate } = param;
      state.date9 = [weekDate[0].date[3], weekDate[1].date[3]];
    };
    const setSelectalue10 = (param: any) => {
      state.disabled10 = param[2] % 2 === 0;
    };

    const clickBtn = () => {
      let date = [Utils.date2Str(new Date()), Utils.getDay(6)];
      state.date5 = date;
    };
    const clickBtn1 = () => {
      let date = new Date();
      let year = date.getFullYear();
      let month: any = date.getMonth() + 1;
      month = month < 10 ? '0' + month : month + '';
      let yearMonth = `${year}-${month}`;
      let currMonthDays = Utils.getMonthDays(year + '', month + '');
      state.date5 = [`${yearMonth}-01`, `${yearMonth}-${currMonthDays}`];
    };
    const goDate = () => {
      if (calendarRef.value) {
        var date1 = new Date();
        date1.setDate(date1.getDate() + 30);
        calendarRef.value.scrollToDate(Utils.date2Str(date1));
      }
    };
    const renderDate = (date: { date: Day }) => {
      return +date.date.day <= 9 ? '0' + date.date.day : date.date.day;
    };
    const clickBtn10 = (dateInfo: any) => {
      state.date10 = dateInfo.date[3];
      state.isVisible10 = false;
    };
    return {
      ...toRefs(state),
      openSwitch,
      closeSwitch,
      setChooseValue,
      setChooseValue1,
      setChooseValue7,
      setChooseValue2,
      setChooseValue3,
      setChooseValue4,
      setChooseValue5,
      setChooseValue6,
      setChooseValue8,
      setChooseValue9,
      setSelectalue10,
      clickBtn,
      clickBtn1,
      goDate,
      calendarRef,
      select,
      translate,
      renderDate,
      clickBtn10
    };
  }
});
</script>

<style lang="scss" scoped>
.demo {
  ::v-deep(.nut-cell__value) {
    flex: initial;
  }
}

.test-calendar-wrapper {
  display: flex;
  width: 100%;
  height: 560px;
  overflow: hidden;
}

.wrapper {
  display: flex;
  padding: 0 40px;
  justify-content: center;
}

.d_div {
  margin: 0px 5px;

  .d_btn {
    background: #fa3f19;
    color: #fff;
    font-size: 12px;
    padding: 2px 8px;
    border-radius: 4px;
    display: inline-block;
    height: 16px;
  }
}

.info {
  font-size: 12px;
  line-height: 14px;
}
</style>
