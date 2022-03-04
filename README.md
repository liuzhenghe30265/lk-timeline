# 带播放功能的时间轴

[![npm](https://img.shields.io/badge/npm-6.4.1-brightgreen.svg)](https://www.npmjs.com/)
[![webpack](https://img.shields.io/badge/webpack-^3.6.0-brightgreen.svg)](https://github.com/webpack/webpack)
[![vue](https://img.shields.io/badge/vue-^2.5.11-brightgreen.svg)](https://github.com/vuejs/)

![](https://tva1.sinaimg.cn/large/0081Kckwly1gl4ocsndfbj31nw0r2n1k.jpg)

[Demo](https://liuzhenghe30265.github.io/lk-timeline/)

## 📦 Install

```
npm install lk-timeline --save
```

### 全局引用

main.js

```
import LkTimeline from 'lk-timeline'
Vue.use(LkTimeline)
```

### 组件内引用

```
import LkTimeline from 'lk-timeline'
export default {
  name: '',
  components: {
    LkTimeline
  },
}
```

## 🔧 Usage

```
    <lk-timeline
      :options="options"
      :dateTimes="dateTimes"
      @getDateFun="getDateFun"
      :interval="interval"
    ></lk-timeline>
```

## Available props

| **参数**  | **类型** | **默认值**                                               | **说明**                                                                                                 |
| :-------- | :------: | :------------------------------------------------------- | :------------------------------------------------------------------------------------------------------- |
| options   |  Object  | options: {speed: 1, // 速度 speedMax: 10 // 速度最大值}, | 默认速度：1， 默认速度最大值 10                                                                          |
| dateTimes |  Array   | []                                                       | 默认最近十天日期，格式 ['03-04','03-05','03-06','03-07','03-08','03-09','03-10','03-11','03-12','03-13'] |
| interval  |  Number  | 100                                                      | 日期间的间隔， 默认 100                                                                                  |

## Events

| **事件名称** |    **说明**    | **回调参数** |
| :----------- | :------------: | :----------- |
| getDateFun   | 时间切换后触发 | 日期         |

# Build Setup

```bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```
