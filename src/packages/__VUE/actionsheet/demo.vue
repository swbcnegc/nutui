<template>
  <div class="demo">
    <h2>{{ translate('basic') }}</h2>
    <nut-cell is-link @click="switchActionSheet('isVisible1')">
      <div>{{ translate('basic') }}</div>
      <div>{{ state.val1 }}</div>
    </nut-cell>
    <nut-action-sheet v-model:visible="state.isVisible1" :menu-items="menuItemsOne" @choose="chooseItem" />

    <nut-cell is-link @click="switchActionSheet('isVisible2')">
      <div>{{ translate('showCancelButton') }}</div>
      <div>{{ state.val2 }}</div>
    </nut-cell>
    <nut-action-sheet
      v-model:visible="state.isVisible2"
      :cancel-txt="translate('cancelTxt')"
      :menu-items="menuItemsOne"
      @choose="chooseItemTwo"
    />

    <nut-cell is-link @click="switchActionSheet('isVisible3')">
      <div>{{ translate('showDescription') }}</div>
      <div>{{ state.val3 }}</div>
    </nut-cell>
    <nut-action-sheet
      v-model:visible="state.isVisible3"
      :title="translate('title')"
      :description="desc"
      :menu-items="menuItemsTwo"
      :cancel-txt="translate('cancelTxt')"
      @choose="chooseItemThree"
    />

    <h2>{{ translate('optionStatus') }}</h2>
    <nut-cell is-link @click="switchActionSheet('isVisible4')">
      <div>{{ translate('optionStatus') }}</div>
      <div>{{ state.val4 }}</div>
    </nut-cell>
    <nut-action-sheet
      v-model:visible="state.isVisible4"
      :cancel-txt="translate('cancelTxt')"
      :menu-items="menuItemsThree"
      :choose-tag-value="chooseTagValue"
      @choose="chooseItemFour"
    />

    <h2>{{ translate('customContent') }}</h2>
    <nut-cell is-link @click="switchActionSheet('isVisible5')">
      <div>{{ translate('customContent') }}</div>
    </nut-cell>
    <nut-action-sheet v-model:visible="state.isVisible5" :title="translate('title')">
      <div class="custom-content">{{ translate('customContent') }}</div>
    </nut-action-sheet>
  </div>
</template>

<script setup lang="ts">
import { computed, reactive } from 'vue';
import NutActionSheet from './index.vue';
import NutCell from '../cell/index.vue';
import { createComponent } from '@/packages/utils/create';
const { translate } = createComponent('action-sheet');
import { useTranslate } from '@/sites/assets/util/useTranslate';
const initTranslate = () =>
  useTranslate({
    'zh-CN': {
      basic: '基础用法',
      showCancelButton: '展示取消按钮',
      showDescription: '展示描述信息',
      optionStatus: '选项状态',
      customContent: '自定义内容',
      cancelTxt: '取消',
      title: '标题',
      optionOne: '选项一',
      optionTwo: '选项二',
      optionThree: '选项三',
      showDesc: '这是一段描述信息',
      checkOption: '选中选项',
      desc: '描述信息',
      disableOption: '禁用选项',
      loadOptions: '加载选项'
    },
    'en-US': {
      basic: 'Basic Usage',
      showCancelButton: 'Show Cancel Button',
      showDescription: 'Show Description',
      optionStatus: 'Option Status',
      customContent: 'Custom Content',
      cancelTxt: 'cancle',
      title: 'title',
      optionOne: 'option one',
      optionTwo: 'option two',
      optionThree: 'option three',
      showDesc: 'This is a description',
      checkOption: 'Check option',
      desc: 'Description',
      disableOption: 'Disable option',
      loadOptions: 'Load options'
    }
  });
interface Item {
  name: string;
  subname?: string;
  color?: string;
  disable?: boolean;
  loading?: boolean;
}
initTranslate();
const state = reactive({
  isVisible1: false,
  isVisible2: false,
  isVisible3: false,
  isVisible4: false,
  isVisible5: false,
  val1: '',
  val2: '',
  val3: '',
  val4: ''
});
const chooseTagValue = computed(() => translate('checkOption'));
const desc = computed(() => translate('showDesc'));
const menuItemsOne = computed(() => [
  {
    name: translate('optionOne')
  },
  {
    name: translate('optionTwo')
  },
  {
    name: translate('optionThree')
  }
]);
const menuItemsTwo = computed(() => [
  {
    name: translate('optionOne')
  },
  {
    name: translate('optionTwo')
  },
  {
    name: translate('optionThree'),
    color: 'red',
    subname: translate('desc')
  }
]);
const menuItemsThree = computed(() => [
  {
    name: translate('checkOption')
  },
  {
    name: translate('disableOption'),
    disable: true
  },
  {
    name: translate('loadOptions'),
    loading: true
  }
]);
const switchActionSheet = (param: 'isVisible1' | 'isVisible2' | 'isVisible3' | 'isVisible4' | 'isVisible5') => {
  state[param] = !state[param];
};

const chooseItem = (itemParams: any) => {
  state.val1 = itemParams.name;
};

function chooseItemTwo(itemParams: Item) {
  state.val2 = itemParams.name;
}
function chooseItemThree(itemParams: Item) {
  state.val3 = itemParams.name;
}
function chooseItemFour(itemParams: Item) {
  state.val4 = itemParams.name;
}
</script>

<style lang="scss" scoped>
.nut-cell {
  justify-content: space-between;
}

.custom-content {
  padding: 10px 10px 160px;
}

.nut-theme-dark {
  .custom-content {
    color: white;
  }
}
</style>
