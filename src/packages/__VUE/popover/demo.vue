<template>
  <div class="demo">
    <h2>{{ translate('title') }}</h2>

    <nut-row type="flex">
      <nut-col :span="8">
        <nut-popover v-model:visible="lightTheme" :list="iconItemList" location="bottom-start" @choose="chooseItem">
          <template #reference>
            <nut-button type="primary" shape="square">{{ translate('light') }}</nut-button>
          </template>
        </nut-popover>
      </nut-col>
      <nut-col :span="8">
        <nut-popover v-model:visible="darkTheme" theme="dark" location="bottom-start" :list="iconItemList">
          <template #reference>
            <nut-button type="primary" shape="square">{{ translate('dark') }}</nut-button>
          </template>
        </nut-popover>
      </nut-col>
    </nut-row>

    <h2>{{ translate('title1') }}</h2>

    <nut-row type="flex">
      <nut-col :span="8">
        <nut-popover v-model:visible="showIcon" theme="dark" :list="itemList">
          <template #reference>
            <nut-button type="primary" shape="square">{{ translate('showIcon') }}</nut-button>
          </template>
        </nut-popover>
      </nut-col>
      <nut-col :span="8">
        <nut-popover v-model:visible="disableAction" :list="itemListDisabled" location="right">
          <template #reference>
            <nut-button type="primary" shape="square">{{ translate('disableAction') }}</nut-button>
          </template>
        </nut-popover>
      </nut-col>
    </nut-row>

    <h2>{{ translate('title2') }}</h2>
    <nut-popover v-model:visible="Customized" location="top-start" custom-class="customClass">
      <template #reference>
        <nut-button type="primary" shape="square">{{ translate('content') }}</nut-button>
      </template>

      <template #content>
        <div class="self-content">
          <div v-for="(item, index) in selfContent" :key="index" class="self-content-item">
            <Service></Service>
            <div class="self-content-desc">{{ item.desc }}</div>
          </div>
        </div>
      </template>
    </nut-popover>

    <h2>{{ translate('title3') }}</h2>

    <nut-cell title="点击查看更多方向" @click="handlePicker"></nut-cell>
    <nut-popup v-model:visible="showPicker" position="bottom">
      <nut-picker
        :columns="columns"
        title=""
        :swipe-duration="500"
        @change="change"
        @confirm="closePicker"
        @close="closePicker"
      >
        <template #top>
          <div class="brickBox">
            <div id="pickerTarget" class="brick"></div>
          </div>
        </template>
      </nut-picker>
    </nut-popup>

    <nut-popover
      v-model:visible="customPositon"
      target-id="pickerTarget"
      :location="curPostion"
      theme="dark"
      :list="positionList"
    >
    </nut-popover>

    <h2>{{ translate('contentTarget') }}</h2>
    <nut-button id="popid" type="primary" shape="square" @click="clickCustomHandle">
      {{ translate('contentTarget') }}
    </nut-button>
    <nut-popover
      v-model:visible="customTarget"
      target-id="popid"
      :list="iconItemList"
      location="top-start"
    ></nut-popover>

    <h2>{{ translate('contentColor') }}</h2>

    <nut-popover v-model:visible="customColor" :list="iconItemList" location="right-start" bg-color="#f00" theme="dark">
      <template #reference>
        <nut-button type="primary" shape="square">{{ translate('contentColor') }}</nut-button>
      </template>
    </nut-popover>
  </div>
</template>
<script lang="ts">
import { reactive, ref, toRefs, h, defineComponent } from 'vue';
import { createComponent, renderIcon } from '@/packages/utils/create';
const { translate } = createComponent('popover');
import { useTranslate } from '@/sites/assets/util/useTranslate';
import { Service, Notice, Location, Category, Scan2, Message, Cart2, My2 } from '@nutui/icons-vue';

const initTranslate = () =>
  useTranslate({
    'zh-CN': {
      title: '基础用法',
      title1: '选项配置',
      title2: '自定义内容',
      title3: '位置自定义',
      light: '明朗风格',
      dark: '暗黑风格',
      showIcon: '展示图标',
      disableAction: '禁用选项',
      content: '自定义内容',
      contentColor: '自定义颜色',
      contentTarget: '自定义目标元素'
    },
    'en-US': {
      title: 'Basic Usage',
      title1: 'Option Configuration',
      title2: 'Custom Content',
      title3: 'Custom Location',
      light: 'light',
      dark: 'dark',
      showIcon: 'show icon',
      disableAction: 'disabled',
      content: 'custom content',
      contentColor: 'custom color',
      contentTarget: 'custom target'
    }
  });
export default defineComponent({
  components: { Service },
  setup() {
    initTranslate();

    const state = reactive({
      showIcon: false,
      placement: false,
      darkTheme: false,
      lightTheme: false,
      Customized: false,
      disableAction: false,
      topLocation: false, //向上弹出
      rightLocation: false, //向右弹出
      leftLocation: false, //向左弹出
      customPositon: false,

      showPicker: false,

      customTarget: false,
      customColor: false
    });
    const curPostion = ref('top');

    const columns = ref([
      { text: 'top', value: 'top' },
      { text: 'top-start', value: 'top-start' },
      { text: 'top-end', value: 'top-end' },
      { text: 'right', value: 'right' },
      { text: 'right-start', value: 'right-start' },
      { text: 'right-end', value: 'right-end' },
      { text: 'bottom', value: 'bottom' },
      { text: 'bottom-start', value: 'bottom-start' },
      { text: 'bottom-end', value: 'bottom-end' },
      { text: 'left', value: 'left' },
      { text: 'left-start', value: 'left-start' },
      { text: 'left-end', value: 'left-end' }
    ]);

    const iconItemList = reactive([
      {
        name: 'option1'
      },
      {
        name: 'option2'
      },
      {
        name: 'option3'
      }
    ]);

    const positionList = reactive([
      {
        name: 'option1'
      },
      {
        name: 'option2'
      }
    ]);

    const itemList = reactive([
      {
        name: 'option1',
        icon: () => {
          return h(My2, {
            width: '14px',
            color: 'rgba(250, 44, 25, 1)'
          });
        }
      },
      {
        name: 'option2',
        icon: () => {
          return h(Cart2, {
            width: '14px'
          });
        }
      },
      {
        name: 'option3',
        icon: () => {
          return h(Location, {
            width: '14px'
          });
        }
      }
    ]);

    const itemListDisabled = reactive([
      {
        name: 'option1',
        disabled: true
      },
      {
        name: 'option2',
        disabled: true
      },
      {
        name: 'option3'
      }
    ]);

    const selfContent = reactive([
      {
        name: Service,
        desc: 'option1'
      },
      {
        name: Notice,
        desc: 'option2'
      },
      {
        name: Location,
        desc: 'option3'
      },
      {
        name: Category,
        desc: 'option4'
      },
      {
        name: Scan2,
        desc: 'option5'
      },
      {
        name: Message,
        desc: 'option6'
      }
    ]);

    const chooseItem = (item: unknown, index: number) => {
      console.log(item, index);
    };

    const handlePicker = () => {
      state.showPicker = true;
      setTimeout(() => {
        state.customPositon = true;
      }, 0);
    };

    const change = ({ selectedValue }) => {
      curPostion.value = selectedValue[0];
      if (state.showPicker) state.customPositon = true;
    };

    const clickCustomHandle = () => {
      state.customTarget = !state.customTarget;
    };

    const closePicker = () => {
      state.customPositon = false;
      state.showPicker = false;
    };

    return {
      iconItemList,
      itemList,
      ...toRefs(state),
      itemListDisabled,
      selfContent,
      chooseItem,
      curPostion,
      positionList,
      translate,
      columns,
      change,
      handlePicker,
      clickCustomHandle,
      renderIcon,
      closePicker
    };
  }
});
</script>
<style lang="scss">
.demo > h2 {
  padding: 0;
}

.brickBox {
  margin: 80px 0;
  display: flex;
  justify-content: center;

  .brick {
    width: 60px;
    height: 60px;
    background: linear-gradient(135deg, #fa2c19 0%, #fa6419 100%);
    border-radius: 10px;
  }
}

.demo {
  .nut-popover-content {
    width: 120px;
  }
}

.customClass {
  .nut-popover-content {
    width: auto;
  }

  .self-content {
    width: 195px;
    display: flex;
    flex-wrap: wrap;

    &-item {
      margin-top: 10px;
      margin-bottom: 10px;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
    }

    &-desc {
      margin-top: 5px;
      width: 60px;
      font-size: 10px;
      text-align: center;
    }
  }
}
</style>
