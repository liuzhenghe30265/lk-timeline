# lk-timeline

> 基于 Vue 开发的带播放功能的时间轴插件

![](https://tva1.sinaimg.cn/large/00831rSTly1gd06qlr8rgg31ao06sdqr.gif)

[github 地址](https://github.com/liuzhenghe30265/lk-timeline)


# Demo

[lk-timeline](http://blog.liuzhenghe.com/lk-timeline/)


# Install

From npm:
```
npm install lk-timeline --save
```

# Usage

## 全局使用

main.js
```
import LkTimeline from 'lk-timeline'
Vue.use(LkTimeline)
```

.vue
```
<template>
  <div>
    <lk-timeline
      :options="options"
      :dateTimes="dateTimes"
      @getDateFun="getDateFun"
      :interval="interval"
    ></lk-timeline>
  </div>
</template>

<script>
export default {
  name: '',
  data() {
    return {
      date: '',
      options: {
        speed: 1, // 速度
        speedMax: 10 // 速度最大值
      },
      interval: 200, // 日期间的间隔
      dateTimes: [
        '03-04',
        '03-05',
        '03-06',
        '03-07',
        '03-08',
        '03-09',
        '03-10',
        '03-11',
        '03-12',
        '03-13'
      ]
    }
  },
  methods: {
    getDateFun(time) {
      console.log(time)
    }
  }
}
</script>
```


## 组件内使用

```
<template>
  <div>
    <lk-timeline
      :options="options"
      :dateTimes="dateTimes"
      @getDateFun="getDateFun"
      :interval="interval"
    ></lk-timeline>
  </div>
</template>

<script>
import LkTimeline from 'lk-timeline'
export default {
  name: '',
  components: {
    LkTimeline
  },
  data() {
    return {
      date: '',
      options: {
        speed: 1, // 速度
        speedMax: 10 // 速度最大值
      },
      interval: 200, // 日期间的间隔
      dateTimes: [
        '03-04',
        '03-05',
        '03-06',
        '03-07',
        '03-08',
        '03-09',
        '03-10',
        '03-11',
        '03-12',
        '03-13'
      ]
    }
  },
  methods: {
    getDateFun(time) {
      console.log(time)
    }
  }
}
</script>
```
