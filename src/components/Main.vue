/*
 * @Author: liuzhenghe
 * @Email: liuzhenghe@btzh.cn
 * @Date: 2020-03-13 10:14:13
 * @Last Modified by: liuzhenghe30265
 * @Last Modified time: 2020-03-20 11:37:51
 * @Description: 带播放功能的时间轴
 */
<template>
  <div class="timeline_main">
    <div class="timeline_control">
      <div class="menu_play">
        <i class="menu_icon icon_left" :class="{'menu_icon_disabled':playing}" @click="backward"></i>
        <i
          class="menu_icon"
          :class="{'icon_play':!playing, 'icon_pause':playing}"
          @click="togglePlay"
          @mouseleave="hoverIndex = -1"
        ></i>
        <i class="menu_icon icon_right" :class="{'menu_icon_disabled':playing}" @click="forward"></i>
      </div>
      <div class="menu_setting">
        <i class="menu_icon icon_up" :class="{'menu_icon_disabled':playing}" @click="speedSlow"></i>
        <i class="speed">{{ options.speed }}</i>
        <i class="menu_icon icon_down" :class="{'menu_icon_disabled':playing}" @click="speedQuick"></i>
      </div>
    </div>
    <div class="timeline_axis">
      <div class="axis_item" v-for="(time, index) in dateTimes" :key="index">
        <div
          class="axis_item_tick"
          :class="{ 'axis_item_tick_active':index === highlightIndex }"
          @mouseenter="hoverIndex = index"
          @mouseleave="hoverIndex = -1"
          @click="tickClick(time, index)"
        ></div>
        <div class="axis_item_label" v-if="dateTimeIndexes.indexOf(index) >=0 ">{{ time }}</div>
        <div class="axis_item_tip" v-if="index === highlightIndex || index === hoverIndex">{{ time}}</div>
      </div>
    </div>
  </div>
</template>
<script>
import { dateFormat } from '../util/formatdate.js' // 日期格式化
export default {
  name: 'lk-timeline',
  data() {
    return {
      intervalTimer: null, // 定时器
      dateTimeIndexes: [], // 日期列表
      playing: false, // 播放
      activeIndex: 0, // 当前的时间位置
      hoverIndex: 0 // 鼠标移入的时间位置
    }
  },
  props: {
    options: {
      type: Object,
      default() {
        return {}
      }
    },
    dateTimes: {
      type: Array,
      default() {
        return []
      }
    },
    interval: {
      type: Number,
      default() {
        return 100
      }
    }
  },
  computed: {
    highlightIndex() {
      return (
        (this.activeIndex === -1 && this.dateTimes.length - 1) ||
        this.activeIndex
      )
    }
  },
  watch: {
    options: {
      handler() {
        this.renderTimeline()
      },
      deep: true
    },
    playing() {
      if (this.playing) {
        this.intervalTimer = setInterval(() => {
          this.activeIndex = (this.activeIndex + 1) % this.dateTimes.length
        }, this.options.speed * 1000)
      } else {
        if (this.intervalTimer) {
          clearInterval(this.intervalTimer)
          this.intervalTimer = null
        }
      }
    },
    activeIndex() {
      const time = this.dateTimes[this.activeIndex].split(' ')[0]
      console.log(time)
      this.$emit('getDateFun', time)
    }
  },
  mounted() {
    this.renderTimeline()
    let that = this
    window.onresize = function() {
      that.renderTimeline()
    }
  },
  filters: {
    formatDatetime(dateTime) {
      dateTime = dateFormat(dateTime, 'MM.dd')
      return dateTime
    }
  },
  methods: {
    // 获取时间
    getDateFun(time) {
      console.log(time)
    },
    // 初始化时间轴
    renderTimeline() {
      // 时间轴的宽度
      const timelineWidth = this.$el.offsetWidth - 40
      // 日期个数
      const dateTimesSize = this.dateTimes.length
      // 如果时间全部显示，时间轴的理想宽度
      const dateTimesWidth = dateTimesSize * this.interval
      // 如果时间轴的宽度小于理想宽度
      if (timelineWidth >= dateTimesWidth) {
        this.dateTimeIndexes = this.dateTimes.map((dateTime, index) => {
          return index
        })
        return
      }
      // 当前时间轴的宽度最大能容纳多少日期刻度
      const maxTicks = Math.floor(timelineWidth / this.interval)
      // 间隔刻度数
      const gapTicks = Math.floor(dateTimesSize / maxTicks)
      // 记录需要显示的日期索引
      this.dateTimeIndexes = []
      for (let t = 0; t <= maxTicks; t++) {
        this.dateTimeIndexes.push(t * gapTicks)
      }
      const len = this.dateTimeIndexes.length
      // 最后一项需要特殊处理
      if (len > 0) {
        const lastIndex = this.dateTimeIndexes[len - 1]
        if (lastIndex + gapTicks > dateTimesSize - 1) {
          this.dateTimeIndexes[len - 1] = dateTimesSize - 1
        } else {
          this.dateTimeIndexes.push(dateTimesSize - 1)
        }
      }
    },
    // 点击刻度
    tickClick(time, index) {
      if (this.playing) {
        return
      }
      this.activeIndex = index
    },
    // 播放和暂停
    togglePlay() {
      this.playing = !this.playing
    },
    // 时间退后一日
    backward() {
      if (this.playing) {
        return
      }
      this.activeIndex = this.activeIndex - 1
      if (this.activeIndex === -1) {
        this.activeIndex = this.dateTimes.length - 1
      }
    },
    // 时间前进一日
    forward() {
      if (this.playing) {
        return
      }
      this.activeIndex = (this.activeIndex + 1) % this.dateTimes.length
    },
    // 减慢速度
    speedSlow() {
      if (this.playing || this.options.speed >= this.options.speedMax) {
        return
      }
      this.options.speed = this.options.speed + 1
    },
    // 加快速度
    speedQuick() {
      if (this.playing || this.options.speed <= 1) {
        return
      }
      this.options.speed = this.options.speed - 1
    }
  }
}
</script>
<style scoped lang="scss">
.timeline_main {
  padding: 10px;
  box-sizing: border-box;
  .timeline_control {
    overflow: hidden;
    margin-bottom: 30px;

    i {
      cursor: pointer;
      display: inline-block;
      font-style: normal;
    }

    .menu_icon {
      font-size: 20px;
      width: 20px;
      height: 20px;
      background-size: cover;
      background-repeat: no-repeat;
    }

    .menu_play {
      color: #00fffa;
      float: left;
    }

    .menu_icon_disabled {
      cursor: no-drop;
      opacity: 0.5;
    }

    .menu_setting {
      overflow: hidden;
      float: left;
      margin-left: 20px;

      i {
        float: left;

        &.speed {
          padding: 0 5px;
        }
      }

      span {
        color: #cee7ff;
      }
    }

    .icon_left {
      // background-image: url('../assets/icon_left.png');
      background-image: url(data:image/png;base64,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);
    }

    .icon_right {
      // background-image: url('../assets/icon_right.png');
      background-image: url(data:image/png;base64,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);
    }

    .icon_play {
      // background-image: url('../assets/icon_play.png');
      background-image: url(data:image/png;base64,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);
    }

    .icon_pause {
      // background-image: url('../assets/icon_pause.png');
      background-image: url(data:image/png;base64,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);
    }

    .icon_up {
      // background-image: url('../assets/icon_up.png');
      background-image: url(data:image/png;base64,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);
    }

    .icon_down {
      // background-image: url('../assets/icon_down.png');
      background-image: url(data:image/png;base64,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);
    }
  }

  .timeline_axis {
    position: relative;
    display: flex;
    justify-content: space-around;
    padding: 8px 0;

    &::before {
      content: '';
      width: 100%;
      height: 10px;
      position: absolute;
      left: 0;
      bottom: 8px;
      display: inline-block;
      background: rgba(0, 0, 0, 0.5);
    }
  }

  .axis_item {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;

    .axis_item_tick {
      display: inline-block;
      width: 4px;
      height: 20px;
      background: rgba(0, 0, 0, 0.5);
      transition: background 0.3s;
      cursor: pointer;

      &:hover {
        background: #00fffa;
      }
    }

    .axis_item_tick_active {
      background: #00fffa;
    }

    .axis_item_label {
      position: absolute;
      bottom: -30px;
      white-space: nowrap;
    }

    .axis_item_tip {
      position: absolute;
      top: -25px;
      padding: 2px 6px;
      border-radius: 2px;
      background: rgba(0, 0, 0, 0.5);
      white-space: nowrap;
      color: #fff;
    }
  }
}
</style>