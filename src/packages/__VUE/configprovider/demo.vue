<template>
  <div class="demo">
    <h2>{{ translate('basic') }}</h2>
    <nut-config-provider :theme="theme">
      <nut-cell :title="translate('title1')">
        <template #link>
          <nut-switch v-model="checked" @change="change" />
        </template>
      </nut-cell>
      <nut-cell :title="translate('title2')" :sub-title="translate('title3')" :desc="translate('desc')"></nut-cell>

      <nut-form>
        <nut-form-item :label="translate('switch')">
          <nut-switch v-model="formData2.switch"></nut-switch>
        </nut-form-item>
        <nut-form-item :label="translate('checkbox')">
          <nut-checkbox v-model="formData2.checkbox">{{ translate('checkbox') }}</nut-checkbox>
        </nut-form-item>
        <nut-form-item :label="translate('radiogroup')">
          <nut-radio-group v-model="formData2.radio" direction="horizontal">
            <nut-radio label="1">{{ translate('option', 1) }}</nut-radio>
            <nut-radio disabled label="2">{{ translate('option', 2) }}</nut-radio>
            <nut-radio label="3">{{ translate('option', 3) }}</nut-radio>
          </nut-radio-group>
        </nut-form-item>
        <nut-form-item :label="translate('rate')">
          <nut-rate v-model="formData2.rate" />
        </nut-form-item>
        <nut-form-item :label="translate('inputnumber')">
          <nut-input-number v-model="formData2.number" />
        </nut-form-item>
        <nut-form-item :label="translate('range')">
          <nut-range v-model="formData2.range" hidden-tag></nut-range>
        </nut-form-item>
        <nut-form-item :label="translate('uploader')">
          <nut-uploader v-model:file-list="formData2.defaultFileList" url="http://apiurl" maximum="3" multiple>
          </nut-uploader>
        </nut-form-item>
        <nut-form-item :label="translate('address')">
          <input
            v-model="formData2.address"
            class="nut-input-text"
            readonly
            :placeholder="translate('addressTip1')"
            type="text"
            @click="addressModule.methods.show"
          />
          <!-- nut-address -->
          <nut-address
            v-model:visible="addressModule.state.show"
            :province="addressModule.state.province"
            :city="addressModule.state.city"
            :country="addressModule.state.country"
            :town="addressModule.state.town"
            :custom-address-title="translate('addressTip2')"
            @close="addressModule.methods.onClose"
          ></nut-address>
        </nut-form-item>
      </nut-form>
    </nut-config-provider>
    <h2>{{ translate('custom') }}</h2>
    <nut-config-provider :theme-vars="themeVars">
      <nut-form>
        <nut-form-item :label="translate('range')">
          <nut-range v-model="formData2.range" hidden-tag></nut-range>
        </nut-form-item>
      </nut-form>
    </nut-config-provider>
  </div>
</template>
<script setup lang="ts">
import { reactive, ref } from 'vue';
import NutConfigProvider from './index.vue';
import NutCell from '../cell/index.vue';
import NutForm from '../form/index.vue';
import NutFormItem from '../formitem/index.vue';
import NutRange from '../range/index.vue';
import NutSwitch from '../switch/index.vue';
import NutRate from '../rate/index.vue';
import NutInputNumber from '../inputnumber/index.vue';
import NutUploader from '../uploader/index.vue';
import NutAddress from '../address/index.vue';
import NutRadio from '../radio/index.vue';
import NutCheckbox from '../checkbox/index.vue';
import NutRadioGroup from '../radiogroup/index.vue';
import { createComponent } from '@/packages/utils/create';
const { translate } = createComponent('configprovider');
import { useTranslate } from '@/sites/assets/util/useTranslate';

const initTranslate = () =>
  useTranslate({
    'zh-CN': {
      basic: '基础用法',
      custom: '定制主题',
      title1: '切换暗黑',
      title2: '我是标题',
      title3: '副标题描述',
      desc: '描述文字',
      address: '地址',
      addressTip: '请输入地址',
      addressTip1: '请选择地址',
      addressTip2: '请选择所在地区',
      remarks: '备注',
      remarksTip: '请输入备注',
      switch: '开关',
      checkbox: '复选框',
      radiogroup: '单选按钮',
      option: (v: string) => '选项' + v,
      rate: '评分',
      inputnumber: '步进器',
      range: '滑块',
      uploader: '文件上传',
      success: '上传成功',
      uploading: '上传中...',
      asyncValidator: '模拟异步验证中'
    },
    'en-US': {
      basic: 'Basic Usage',
      custom: 'Custom Theme',
      title1: 'Switch Dark Mode',
      title2: 'Title',
      title3: 'Subtitle Description',
      desc: 'Description',
      nameTip: 'Please enter your name',
      address: 'Address',
      addressTip: 'Please enter address',
      addressTip1: 'Please select an address',
      addressTip2: 'Please select your region',
      remarks: 'Remarks',
      remarksTip: 'Please enter remarks',
      switch: 'Switch',
      checkbox: 'Checkbox',
      radiogroup: 'Radiogroup',
      option: (v: string) => 'Option' + v,
      rate: 'Rate',
      inputnumber: 'Inputnumber',
      range: 'Range',
      uploader: 'Upload file',
      success: 'Upload successful',
      uploading: 'Uploading',
      asyncValidator: 'Simulating asynchronous verification'
    }
  });

initTranslate();
const checked = ref(false);
const theme = ref('');
const change = (v: boolean) => {
  theme.value = v ? 'dark' : '';
};
const formData2 = reactive({
  switch: false,
  checkbox: false,
  radio: 0,
  number: 0,
  rate: 3,
  range: 30,
  address: '',
  defaultFileList: [
    {
      name: 'file 1.png',
      url: 'https://m.360buyimg.com/babel/jfs/t1/164410/22/25162/93384/616eac6cE6c711350/0cac53c1b82e1b05.gif',
      status: 'success',
      message: translate('success'),
      type: 'image'
    },
    {
      name: 'file 2.png',
      url: 'https://m.360buyimg.com/babel/jfs/t1/164410/22/25162/93384/616eac6cE6c711350/0cac53c1b82e1b05.gif',
      status: 'uploading',
      message: translate('uploading'),
      type: 'image'
    }
  ]
});

const addressModule = reactive({
  state: {
    show: false,
    province: [
      { id: 1, name: '北京' },
      { id: 2, name: '广西' },
      { id: 3, name: '江西' },
      { id: 4, name: '四川' }
    ],
    city: [
      { id: 7, name: '朝阳区' },
      { id: 8, name: '崇文区' },
      { id: 9, name: '昌平区' },
      { id: 6, name: '石景山区' }
    ],
    country: [
      { id: 3, name: '八里庄街道' },
      { id: 9, name: '北苑' },
      { id: 4, name: '常营乡' }
    ],
    town: []
  } as any,
  methods: {
    show() {
      addressModule.state.show = !addressModule.state.show;
      if (addressModule.state.show) {
        formData2.address = '';
      }
    },
    onClose({ data }: any) {
      formData2.address = data.addressStr;
      addressModule.state.show = false;
    }
  }
});
const themeVars = reactive({
  primaryColor: '#008000',
  primaryColorEnd: '#008000'
});
</script>
