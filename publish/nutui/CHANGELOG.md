## v4.2.0

`2023-10-27`

New Components

- 新增 AvatarCropper 头像裁剪组件 (#2570) @yi-boide
- 新增 Space 布局组件 (#2584) @yi-boide

Features

- build: 新增 css 样式产物可选 (#2602) @yi-boide
- build: resolver 配置独立为 @nutui/auto-import-resolver 包 (#2602) @yi-boide
- collapse: 增加自定义插槽 icon (#2588) (#2612) @yi-boide

Bug Fixes

- input-number: 修复超过 min, max 范围问题 (#2606) (#2607) @Formulaaa
- picker: 修复 columns 为空时不触发更新的问题 (#2590) (#2611) @yi-boide
- circle-progress: 修复数值变化在 iOS 下的 border 闪烁问题（#2481） (#2619) @yi-boide
- comment: 追评中图片增加响应事件 (#2448) (#2620) @yi-boide
- ellipsis: fix docs typo (#2616) @yang1206

BREAKING CHANGES

- !input: Taro 版组件 type 属性的 `number`、`digit` 与小程序原生 API 对齐，支持调起数字、带小数点键盘 (#2578) @mikasayw

## v4.1.7

`2023-10-13`

Features

- searchbar: 增加 cursor-spacing 属性(#2458) (#2587) @yi-boide

Bug Fixes

- image-preview: 修复图片放大不能滑动 (#2136) (#2574) @yi-boide
- radio: 修复 radio-group 的 text-position 属性响应式问题 (#2572) @mikasayw
- radio: 修复按钮类型激活背景边框样式问题 (#2579) @eiinu
- textarea: 修复 readonly 时换行符的显示问题（#2566）(#2583) @yi-boide
- tabs: 修复 disabled 情况下能滑动过去的问题(#2486) (#2559) @yi-boide
- checkbox/input-number: 调整为内联元素 (#2581) @yi-boide
- form: use subtree to validate nut-form (#2582) @mikasayw
- form: validate 获取子节点规则方式调整 (#2593) @eiinu

## v4.1.6

`2023-09-19`

Features

- calendar: 日历底部增加插槽 (#2550) @Arch94
- cell: 新增 desc slot (#2503) (#2556) @yi-boide
- searchbar: 增加safe-area-inset-bottom 属性 (#2265) (#2558) @yi-boide
- video: 增加播放，暂停，停止，静音，取消静音方法 (#2447) (#2555) @yi-boide

Bug Fixes

- searchbar: 修复抖音小程序下输入抖动的问题 (#2552) @junjun666
- uploader: 修复安卓环境下无法唤起摄像头问题 (#2567) @developeryvan
- configprovider: 修复 slot 的响应式问题 @eiinu

## v4.1.5

`2023-09-05`

Bug Fixes

- countdown: millisecond format (#2514) @subordon
- calendar: 修复周选择时 weekDate 相关报错 (#2538) @iCheng
- calendar: 修正 demo 默认文案显示条件 (#2539) @iCheng
- fix: 部分组件增加 catch-move 阻止滚动穿透 (#2543)
- fix: countdown/invoice/navbar 优化事件命名 (#2536)

Refactor

- configprovider: move common logic to common file (#2540) @subordon

## v4.1.4

`2023-08-25`

Features

- picker: 支持自定义 column fieldNames (#2460) @subordon

Bug Fixes

- popup: 修复内容无法滑动问题
- form: 调整 label 中 min-width 样式权重 (#2478)
- addresslist: update docs (#2495) @subordon
- actionsheet: 修复 close-abled 失效问题 (#2496)
- picker: 修复 field-names 在级联模式下的问题
- 修复组件内 scroll-view 标签在 taro-h5 端的兼容问题 (#2470)

## v4.1.3

`2023-08-04`

Bug Fixes

- calendar: 修复自定义起始日高亮列错误问题 (#2452)
- cascader: 修复在 Popup 中的滑动问题 (#2465)
- input: 修复空白节点导致的样式对齐问题 (#2473)
- taro 端组件根元素统一为 view 标签 (#2467)
- type: 修复部分组件事件类型丢失问题 (#2469)
- docs: 组件文档 demo 代码走查修改 (#2459)

## v4.1.2

`2023-07-24`

Bug Fixes

- price: 修复小程序下符号转义丢失问题

## v4.1.1

`2023-07-21`

Features

- swipe: 新增 swipe-group 组件用于控制 swipe 操作互斥 (#2439)
- animate: 新增 show 属性与 animate 事件 (#2434)
- uploader: 小程序下新增预览图片 mode 属性 (#2435)

Bug Fixes

- utils: useTaroRect 方法增加异常处理 (#2431)
- button: 优化边框与渐变色背景样式问题
- type: 修复部分组件类型文件丢失问题

## v4.1.0

NutUI 非常感谢您对开源事业做出的贡献！
本次社区贡献者 @thx125 @wenjuGao @subordon @yang1206 @aooiuu

`2023-07-14`

Features

- 新增 `@nutui/touch-emulator`
- 新增 `@nutui/playground`
- form: 支持异步返回的自定义校验文案 (#2380) @subordon
- cell: 新增 box-shdow 样式变量 (#2382)
- calendar: 支持透传 Popup 属性 (#2410)
- address: 新增 round 属性 (#2411)

Bug Fixes

- address: 修复4级地址不触发 onchange 事件 (#2389) @thx125
- avatar: 修复显示头像折叠元素有错误问题 (#2417) @thx125
- calendar: 修复平铺展示时丢失 ref 问题 (#2390) @wenjuGao
- signature: 修复 Taro H5 getRect 方法异常
- sticky: 重构组件，修复 sticky 失效问题 (#2393)
- form: 获取子节点方法增加兜底处理，修复 JSX 下报错问题
- imagepreview: replace var with const (#2423) @subordon
- uploader: 支持异步修改 file-list (#2421)
- textarea: docs 文档修改 (#2428) @aooiuu
- component class name typo (#2392) @yang1206
- fix case typo (#2429) @subordon

## v4.0.12

`2023-06-27`

* :bug: fix(build): 构建 Taro 包时关闭 minify，修复 WEAPP 找不到模板问题
* :bug: fix(image): 修复 iOS 下懒加载异常问题 #2376

## v4.0.11

`2023-06-26`

* :bug: fix(build): 修复 CDN(UMD) 方式使用图标丢失问题

## v4.0.10

`2023-06-25`

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷
本次社区贡献者 @alphardex @tolking

* :zap: feat(AddressList): slot 作用域增加列表项数据 (#2372)
* :bug: fix(Picker): strictly compare the type of value #2341 (#2342) @tolking
* :bug: fix(Uploader): 修复微信小程序上传图片时 size-type 参数无效的问题 (#2352) @alphardex
* :bug: fix(Searchbar): 修复 shape 设置无效的问题 (#2346)
* :bug: fix(Layout): 优化样式逻辑 (#2362)
* :bug: fix(Menu): 修复 menu 点击闪屏问题 (#2364)
* :bug: fix(Icon): 修复 click 事件多次触发的问题
* :rocket: chore: 增加 ESM 全量构建产物 (#2363)
* :rocket: chore: icons-vue 包更新至 0.1.1 版本

## v4.0.9

`2023-06-09`

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷
本次社区贡献者 @wenfangdu @blue0728 @zhousg

* :bug: fix(popup): 修复未开启 overlay 时滚动穿透失效问题 (#2311) @wenfangdu
* :bug: fix(uploader): 修改 Taro-h5 下预览图片显示逻辑
* :bug: fix(imagepreview): 函数调用结束移除背景锁定样式 (#2326)
* :bug: fix(imagepreview): 修复 content-close 失效问题 (#2330)
* :bug: fix(dialog): 修复 close-on-click-overlay 失效问题 (#2337)
* :bug: fix(tabs): 修复 vertical 方式下 title-gutter 方向错误 (#2339)
* :bug: fix(collapse): 修复未设置 v-model 时无法触发事件问题 (#2340)
* :bug: fix: fix code comments (#2328) @zhousg
* :rocket: chore: bumped unplugin-vue-components peer dep (#2331) @wenfangdu
* :rocket: chore: vite 构建产物兼容 es2015 (#2322) @blue0728
* :rocket: chore(vsce): bump to pnpm (#2315)

## v4.0.8

`2023-05-26`

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷
本次社区贡献者 @tolking

* :zap: feat(image): 添加 lazy-load 属性 (#2295)
* :bug: fix(checkbox): fix the icon display about indeterminate (#2301) @tolking
* :bug: fix(collapse): 修复异步加载数据时无法自动展开问题 (#2302)
* :bug: fix(noticebar): 修复支付宝小程序下横向动画失效问题 (#2306)
* :bug: fix(checkbox): 修复组件销毁时 v-model 数据改变的问题 (#2308)
* :bug: chore(taro): 移除多余的组件导出，修复字节小程序编译警告 (#2309)
* :rocket: chore(style): 调整样式依赖关系生成方式 (#2302)

## v4.0.7

`2023-05-12`

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷
本次社区贡献者 @ntnyq

* :zap: feat(animate): add duration property (#2280)
* :zap: feat(button): add formType for Taro H5 (#2288)
* :bug: fix(switch): inactiveValue 为非 falsy 值无法显示 (#2258)
* :bug: fix(tabs): tab-title active font-weight #2268
* :bug: fix(sidenavbar): style of async data (#2279)
* :bug: fix(addressList): failed to emit click-item (#2281)
* :bug: fix(form): reset min-width style for label-width (#2282)
* :bug: fix(tour): remove mask after close (#2283)

## v4.0.6

`2023-04-21`

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷
本次社区贡献者 @kingyang @blue0728

* :zap: feat(input): add focus, blur, select ref (#2250)
* :bug: fix(input): 传入 class 被忽略问题 #2242 (#2243) @kingyang
* :bug: fix(Menu): 修复 MenuItem class 设置无效问题 (#2236) @blue0728
* :bug: fix(datepicker): 修复小程序下 v-model 修改异常问题
* :bug: fix(checkbox): fix toggle all demo #2237
* :bug: fix(rate): 修复定位方式导致的异常空白高度问题 (#2255)
* :bug: fix(picker): 修复平铺模式下溢出省略失效问题 (#2247)
* :bug: fix(dialog): remove hidden class after close #2211 (#2241)
* :rocket: chore: upgrade Taro to 3.6.2+
* :rocket: chore: move yarn to pnpm for Taro Demo

## v4.0.5

`2023-04-07`

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷
本次社区贡献者 @gyt95 @wh-if

* :zap: feat(tabs): support vertical automatic scrolling (#2199) @gyt95
* :zap: feat(datepicker): 支持选择时分 #2167
* :bug: fix(checkbox): checkbox 减少时 group 内数据同步更新 (#2190) #2189 @wh-if
* :bug: fix(tabs): 修复最后一项展示不全的问题 #2214
* :bug: fix(tabs): 限制 tabs 导航滚动方向 #2208
* :bug: fix(input): 修复透传原生属性后 click 失效的问题
* :bug: fix(datepicker): 修复手动修改 modelValue 无效问题 #2205

## v4.0.4

`2023-03-24`

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷
本次社区贡献者 @ntnyq @hlbj105 

* :zap: feat(picker): 支持透传 picker-view 属性 #2142 #2143 #2161 @eiinu
* :bug: fix(tabbar): taro remove href props @Drjingfubo
* :bug: fix: make peer dep optional (#2182) @ntnyq
* :bug: fix(elevator): 修复 iOS 上错误滑动到底部的问题 #2149 @eiinu
* :bug: fix(pullRefresh): 修复 @refresh 回调两次的问题 (#2153) @hlbj105
* :bug: fix(input): 修复 input 校验格式化错误 #2178 (#2193) @eiinu
* :bug: fix(tabs): 修复京东小程序下 tab 栏布局错误 @eiinu
* :bug: fix(dialog): 标题暗黑模式适配修复 @eiinu

## v4.0.3

`2023-03-10`

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷
本次社区贡献者 @daofeng-1998 @n0liu @blachworld

* :zap: feat(tabs): add props swipeable 支持手势滑动切换，优化小程序端切换动画 #1984 #1828 @richard1015
* :zap: feat(noticebar): 增加横向滚动结束时回调事件 (#2135) @daofeng-1998
* :zap: upd(overlay): 优化组件closeOnClickOverlay属性功能 @szg2008
* :zap: chore: 移除 canvas 依赖，替换为 jest-canvas-mock (#2160) @eiinu
* :bug: fix(input): taro input type=number 类型问题修复 (#2171) @lkjh3214
* :bug: fix(list): demo样式修复 @szg2008
* :bug: fix(price): add new prop strike-through @Drjingfubo
* :bug: fix(navbar): change placeholder style @Drjingfubo
* :bug: fix(usetarorect): method modification import @Drjingfubo
* :bug: fix(tabs): web env bug @richard1015
* :bug: fix(tabs): clear log @richard1015
* :bug: fix: list 虚拟列表 列表少渲染一个item (#2152) @blachworld
* :bug: fix(input): 修复支付宝小程序 idcard 类型输入问题 @eiinu
* :bug: fix: 修复 popup 中嵌套 input 高度为 0 的问题 #2144 @eiinu
* :bug: fix(configprovider): v4 移除字体图标相关功能 #2165 (#2166) @eiinu
* :bug: fix: input、textarea 原生标签增加透传属性 @eiinu
* :bug: fix: 修复 resolver 导入问题 #2157 @eiinu
* :bug: fix: menu 闪动问题处理 @Ymm0008
* 📖 docs: tabs数量多,滚动操作 doc 更新 (#2158) @n0liu

## v4.0.2

`2023-02-24`

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷

* :zap: feat(list): 优化虚拟列表，支持不固定高度 #1658 #1382 @szg2008
* :zap: feat(uploader): 微信小程序支持上传视频文件、录制时长等功能 #2119 @richard1015
* :zap: feat(address): 地址组件新增功能 #2101 (#2113) @yangxiaolu1993
* :zap: feat(noticebar): 公告栏组件添加省略号功能 (#2114)  @yangxiaolu1993
* :zap: feat: input 新增 blur 后清除按钮是否展示字段  @lkjh3214
* :bug: fix(tabs): change event emit bug #1884 @richard1015
* :bug: fix(shortpassword): theme-dark style optimization @Drjingfubo
* :bug: fix(form): modify validation rules @Drjingfubo
* :bug: fix(upload): repair type error @Drjingfubo
* :bug: fix(components): remove console @Drjingfubo
* :bug: fix(menu): 修复 menu 收起无动画效果 @Ymm0008
* :bug: fix(signature): signature优化 @Ymm0008
* :bug: fix(barrage): loop模式优化 @Ymm0008
* :bug: fix(input): 修复特殊场景下无法输入的问题 #2123 @eiinu
* 📖 docs(notify): update props table @eiinu 
* 📖 docs(category): fix link of categoryData.js @eiinu 
* 📖 docs: remove scoped in doc.taro.md@eiinu 
* 📖 docs(category): update demo code @eiinu 
* 📖 docs(swiper): taro docs del scoped style @eiinu 
* 📖 docs(inputnumber): modify slot name @Drjingfubo 






## v4.0.1

`2023-02-10`

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷
本次社区贡献者 @dongj0316 @mikasayw @rocwong-cn @thx125 @gyt95

* :zap: feat(checkbox): add checkbox new props of shape (#1978) @mikasayw
* :zap: feat(tabs): title bar supports automatic scrolling (#2035) @gyt95
* :zap: feat: calendar 添加自定义方法 (#2084) @lkjh3214
* :zap: feat: 增加 unplugin 插件配置导出 (#2097)  @eiinu
* :zap: feat: 日历组件新增 周选择功能 (#2102)   @lkjh3214
* :zap: feat(input): 新增slots (#2098)  @lkjh3214
* :zap: feat(badge): 新增bubble样式 (#2091) @dongj0316
* :bug: fix(list): list 虚拟列表未设置height，滚动加载有问题 #2088 @szg2008
* :bug: fix(list): 虚拟列表在ios手机上，滚动条滚动到顶部会出现白屏。 #2031 @szg2008
* :bug: fix: barrage，sinature demo修改 @Ymm0008
* :bug: fix(address): 地址问题修改 (#2054)  @yangxiaolu1993
* :bug: fix: drag，configprovide，badge 修改demo @Drjingfubo
* :bug: fix: taro h5 demo修改 @szg2008
* :bug: fix: jdt-style error @Drjingfubo
* :bug: fix: sku组件修改 (#2056)  @yangxiaolu1993
* :bug: fix: collapse,textarea 优化  @Ymm0008
* :bug: fix(tour): demo style edit @Drjingfubo
* :bug: fix(menu): change style @Drjingfubo
* :bug: fix(calendar): demo样式修改，文档修改 (#2052) @lkjh3214
* :bug: fix(config-provider): 覆盖nut-icon属性 (#2022) @thx125
* :bug: fix(table): demo optimization @Drjingfubo
* :bug: fix: imagepreview问题修改 (#2068) @yangxiaolu1993
* :bug: fix: 修复uploader组件的is-deletable在列表模式下不生效 (#2073) @rocwong-cn
* :bug: fix(input): 事件触发修改 (#2081) @lkjh3214
* :bug: fix(notify): resolve teleport warn @Drjingfubo
* :bug: fix(uploader): 修复图片拉伸样式问题 #2026 @richard1015
* :bug: fix: change sideEffects & default export (#2094) @eiinu
* :bug: fix: 公告栏 noticebar 插槽使用时不更新问题修改 (#2104) @yangxiaolu1993
* :bug: fix: 公告栏demo修改 (#2105) @yangxiaolu1993
* :bug: fix: timer ts error resolve @szg2008
* :bug: fix: 签名组件交互优化 @Ymm0008
* 📖 docs(addresslist): taro env use edit  @richard1015
* 📖 docs: popover修改 (#2053) @yangxiaolu1993
* 📖 docs: price 文档问题 (#2064)  @ailululu
* 📖 docs: card edit @Drjingfubo
* 📖 docs(tabbar): taro add punctuation @Drjingfubo
* 📖 docs: 新增4.0介绍 (#2086)  @yangxiaolu1993

## v4.0.0 🚀

`2023-01-12`

### 新特性

- :zap: 全新的升级，基于 vite4 构建
- :sparkles: 轻量化 安装体积由 14MB 下降至 8MB，减少了 39%
- :art: 支持动态定制主题
- :sparkles: 专属图标库`nutui-icons`
- :sparkles: 支持`unplugin-vue-components`插件
- :sparkles: 新增组件`Tour``PullRefresh`

> 完整内容请移步：「[4.0更新介绍](https://nutui.jd.com/next)」

小程序版本依赖 Taro ^3.5.10

NutUI 非常感谢您对开源事业做出的贡献！🌷🌷🌷

本次社区贡献者 @kasonyang @gyt95 @thx125 @Coffee-C @sjx12 @mikasayw @jxt776

### ⚠️ 升级必读

- [从 v3 升级到 v4](https://nutui.jd.com/4x/index.html#/zh-CN/guide/migrate-from-v3)
