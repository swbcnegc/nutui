<template>
  <div ref="videocon" class="nut-video">
    <video
      ref="root"
      class="nut-video-player"
      :muted="options.muted"
      :autoplay="options.autoplay"
      :loop="options.loop"
      :poster="options.poster"
      :controls="options.controls"
      :preload="options.preload"
      @error="handleError"
    >
      <source :src="source.src" :type="source.type" />
    </video>
    <div v-if="showToolbox && !isDisabled" ref="touchMask" class="nut-video-mask" @click="play"></div>
    <div
      v-if="showToolbox && !isDisabled"
      v-show="!state.playing"
      ref="palyBtn"
      class="nut-video-play-btn"
      @click="play"
    ></div>
    <div
      v-show="showToolbox && !isDisabled"
      class="nut-video-controller"
      :class="{ 'nut-video-controller--show': !state.playing, 'nut-video-controller--hide': state.playing }"
    >
      <div class="nut-video-controller__playbtn" @click="play"></div>
      <div class="nut-video-controller__now">{{ videoSet.displayTime }}</div>
      <div class="nut-video-controller__progress">
        <div ref="progressBar" class="nut-video-controller__progress-value">
          <div class="buffered" :style="{ width: `${videoSet.loaded}%` }"></div>
          <div
            class="nut-video-controller__ball"
            :style="{
              transform: `translate3d(${videoSet.progress.current}px, -50%, 0)`
            }"
            @touchmove.stop.prevent="touchSlidMove($event)"
            @touchstart.stop="touchSlidSrart()"
            @touchend.stop="touchSlidEnd($event)"
          >
            <div class="nut-video-controller__ball-move"></div>
          </div>
          <div ref="playedBar" class="nut-video-controller__played"></div>
        </div>
      </div>
      <div class="nut-video-controller__total">{{ videoSet.totalTime }}</div>
      <div class="nut-video-controller__volume" :class="{ muted: state.isMuted }" @click="handleMuted"></div>
      <div class="nut-video-controller__full" @click="fullScreen"></div>
    </div>
    <!-- 错误弹窗 -->
    <div v-show="state.isError" class="nut-video-error">
      <p class="nut-video-error-tip">{{ translate('errorTip') }}</p>
      <p class="nut-video-error-retry" @click="retry">{{ translate('clickRetry') }}</p>
    </div>
  </div>
</template>
<script lang="ts">
import { computed, reactive, ref, toRefs, watch, nextTick, onMounted, Ref } from 'vue';
import { createComponent } from '@/packages/utils/create';
import { throttle } from '@/packages/utils/throttle';
const { create, translate } = createComponent('video');

export default create({
  props: {
    source: {
      type: Object,
      default: {}
    },
    options: {
      type: Object,
      default: {
        autoplay: false, //是否自动播放
        volume: 0.5,
        poster: '',
        loop: false,
        controls: true,
        muted: false, //是否静音
        disabled: false, //禁止操作
        playsinline: false, //行内展示
        touchPlay: false,
        preload: ''
      },
      required: true
    },
    model: {
      type: String,
      default: ''
    }
  },
  components: {},
  emits: ['click', 'play', 'pause', 'playend', 'time'],

  setup(props, { emit, expose }) {
    const state = reactive({
      videoElm: null,
      initial: true, //控制封面的显示
      showToolbox: false, //控制控制器和标题的显示
      // 视频容器元素
      player: {
        $player: null,
        pos: null
      },
      // progress进度条元素
      progressBar: {
        progressElm: null, // 进度条DOM对象
        pos: null
      },
      // video控制显示设置
      videoSet: {
        loaded: 0, // 缓存长度
        displayTime: '00:00', // 进度时间
        totalTime: '00:00', // 总时间
        progress: {
          width: 0, // 进度条长度
          current: 0 // 进度条当前位置
        }
      },
      state: {
        controlShow: true,
        vol: 0.5, //音量
        currentTime: 0, //当前时间
        fullScreen: false,
        playing: false, //是否正在播放
        isLoading: false,
        isEnd: false,
        isError: false,
        isMuted: false
      },
      showTouchMask: false
    });
    const root = ref(null) as Ref;
    const isDisabled = computed(() => {
      return props.options.disabled;
    });

    watch(
      () => props.source,
      (newValue) => {
        if (newValue.src) {
          nextTick(() => {
            (state.videoElm as any).load();
          });
        }
      },
      { immediate: true, deep: true }
    );

    watch(
      () => props.options,
      (newValue) => {
        state.state.isMuted = newValue ? newValue.muted : false;
      },
      { immediate: true }
    );
    const init = () => {
      (state.videoElm as any) = root.value;

      if (props.options.autoplay) {
        setTimeout(() => {
          (state.videoElm as any).play();
        }, 200);
      }

      if (props.options.touchPlay) {
        state.showTouchMask = true;
      }

      if (props.options.playsinline) {
        (state.videoElm as any).setAttribute('playsinline', props.options.playsinline);
        (state.videoElm as any).setAttribute('webkit-playsinline', props.options.playsinline);
        (state.videoElm as any).setAttribute('x5-video-player-type', 'h5-page');
        (state.videoElm as any).setAttribute('x5-video-player-fullscreen', false);
      }
      volumeHandle();

      if (state.showToolbox) {
        customerInit();
      } else {
        (state.videoElm as any).addEventListener('play', () => {
          state.state.playing = true;
          emit('play', state.videoElm as any);
        });
        (state.videoElm as any).addEventListener('pause', () => {
          state.state.playing = false;
          emit('pause', state.videoElm as any);
        });
        (state.videoElm as any).addEventListener('ended', playEnded);

        (state.videoElm as any).addEventListener('timeupdate', throttle(getPlayTime, 1000));
      }
    };

    const customerInit = () => {
      const $player = root.value;
      const $progress = root.value!.getElementsByClassName('nut-video-controller__progress-value')[0];
      // 播放器位置
      (state.player.$player as any) = $player;
      (state.progressBar.progressElm as any) = $progress;
      (state.progressBar.pos as any) = $progress.getBoundingClientRect();
      state.videoSet.progress.width = Math.round($progress.getBoundingClientRect().width);
    };

    const play = () => {
      if (props.options.autoplay && props.options.disabled) {
        state.state.playing = true;
        return false;
      }
      state.state.playing = !state.state.playing;
      if (state.videoElm) {
        // 播放状态
        if (state.state.playing) {
          try {
            setTimeout(() => {
              (state.videoElm as any).play();
            }, 200);

            // 监听缓存进度
            (state.videoElm as any).addEventListener('progress', () => {
              getLoadTime();
            });
            // 监听播放进度
            (state.videoElm as any).addEventListener('timeupdate', throttle(getPlayTime, 1000));
            // 监听结束
            (state.videoElm as any).addEventListener('ended', playEnded);
            emit('play', state.videoElm);
          } catch (e) {
            // 捕获url异常出现的错误
            handleError();
          }
        }
        // 停止状态
        else {
          (state.videoElm as any).pause();
          emit('pause', state.videoElm);
        }
      }
    };
    const timeFormat = (t: number) => {
      var h = Math.floor(t / 3600) as string | number;
      if (+h < 10) {
        h = '0' + h;
      }
      var m = Math.floor((t % 3600) / 60) as string | number;
      if (+m < 10) {
        m = '0' + m;
      }
      var s = Math.round((t % 3600) % 60) as string | number;
      if (+s < 10) {
        s = '0' + s;
      }
      var str = '';
      if (h != 0) {
        str = h + ':' + m + ':' + s;
      } else {
        str = m + ':' + s;
      }
      return str;
    };

    const getLoadTime = () => {
      if (state.videoSet.loaded)
        state.videoSet.loaded = ((state.videoElm as any).buffered.end(0) / (state.videoElm as any).duration) * 100;
    };

    const getPlayTime = () => {
      const percent = (state.videoElm as any).currentTime / (state.videoElm as any).duration;
      state.videoSet.progress.current = Math.round(state.videoSet.progress.width * percent);

      // 赋值时长
      state.videoSet.totalTime = timeFormat((state.videoElm as any).duration);
      state.videoSet.displayTime = timeFormat((state.videoElm as any).currentTime);
      emit('time', state.videoSet.displayTime, state.videoSet.totalTime);
    };

    const playEnded = () => {
      state.state.playing = false;
      state.state.isEnd = true;
      state.videoSet.displayTime = '00:00';
      state.videoSet.progress.current = 0;
      (state.videoElm as any).currentTime = 0;
      emit('playend', state.videoElm);
    };

    const handleError = () => {
      state.state.isError = true;
    };

    const volumeHandle = () => {
      state.state.vol = props.options.volume;
    };

    const handleMuted = () => {
      state.state.isMuted = !state.state.isMuted;
      (state.videoElm as any).muted = state.state.isMuted;
    };

    const touchSlidSrart = () => {};

    const touchSlidMove = (e: any) => {
      let currentX = e.targetTouches[0].pageX;
      let offsetX = currentX - (state.progressBar.pos as any).left;
      // 边界检测
      if (offsetX <= 0) {
        offsetX = 0;
      }

      if (offsetX >= state.videoSet.progress.width) {
        offsetX = state.videoSet.progress.width;
      }
      state.videoSet.progress.current = offsetX;

      let percent = state.videoSet.progress.current / state.videoSet.progress.width;
      (state.videoElm as any).duration && setPlayTime(percent, (state.videoElm as any).duration);
    };

    const touchSlidEnd = (e: any) => {
      let currentX = e.changedTouches[0].pageX;
      let offsetX = currentX - (state.progressBar.pos as any).left;
      state.videoSet.progress.current = offsetX;
      // 这里的offsetX都是正数
      let percent = offsetX / state.videoSet.progress.width;
      (state.videoElm as any).duration && setPlayTime(percent, (state.videoElm as any).duration);
    };

    const setPlayTime = (percent: number, totalTime: number) => {
      (state.videoElm as any).currentTime = Math.floor(percent * totalTime);
    };

    const retry = () => {
      // console.log('error');
      state.state.isError = false;
      init();
    };

    const fullScreen = () => {
      if (!state.state.fullScreen) {
        state.state.fullScreen = true;
        (state.videoElm as any).webkitRequestFullScreen();
      } else {
        state.state.fullScreen = false;
        (document as any).webkitCancelFullScreen();
      }
    };

    const pause = () => {
      state.state.playing = false;
      (state.videoElm as any).pause();
      emit('pause', state.videoElm as any);
    };

    const stop = () => {
      playEnded();
      (state.videoElm as any).pause();
    };

    const muted = () => {
      state.state.isMuted = true;
      (state.videoElm as any).muted = true;
    };

    const unmuted = () => {
      state.state.isMuted = false;
      (state.videoElm as any).muted = false;
    };

    expose({
      play,
      pause,
      stop,
      muted,
      unmuted
    });

    onMounted(() => {
      init();
    });

    return {
      root,
      ...toRefs(props),
      ...toRefs(state),
      handleError,
      isDisabled,
      play,
      handleMuted,
      touchSlidSrart,
      touchSlidMove,
      touchSlidEnd,
      retry,
      fullScreen,
      translate
    };
  }
});
</script>
