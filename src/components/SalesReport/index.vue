<template>
  <div class="sales-report">
    <div class="header">电商商品销售趋势图</div>
    <div class="content">
      <div class="content-title-wrapper">月销售增长率</div>
      <div class="content-index-wrapper">
        <span class="arrow-up">
          <svg data-v-666af9e5="" aria-hidden="true" focusable="false" data-prefix="fas" data-icon="angle-up" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512" class="svg-inline--fa fa-angle-up fa-w-10"><path data-v-666af9e5="" fill="currentColor" d="M177 159.7l136 136c9.4 9.4 9.4 24.6 0 33.9l-22.6 22.6c-9.4 9.4-24.6 9.4-33.9 0L160 255.9l-96.4 96.4c-9.4 9.4-24.6 9.4-33.9 0L7 329.7c-9.4-9.4-9.4-24.6 0-33.9l136-136c9.4-9.5 24.6-9.5 34-.1z"></path></svg>
        </span>
        <span class="percentage">34<span>%</span></span>
        <span class="text">+14,400</span>
      </div>
      <div id="content-chart" />
      <div class="content-circle-wrapper">
        <div 
          :class="['circle', selectedIndex === index ? 'selected' : '']" 
          v-for="(item, index) in circle" 
          :key="index"
          @click="change(index)"
        />
      </div>
      <div class="content-bottom-wrapper">销售趋势</div>
    </div>
    <div class="footer">
      <div class="footer-wrapper">
        <div class="left">
          <div class="footer-title">订单销售额</div>
          <div class="footer-sub-title">3月累计销售额</div>
        </div>
        <div class="right">
          <small>¥</small> 300,254.00
        </div>
      </div>
      <div class="progress-wrapper">
        <div class="progress-bg">
          <div class="progress-current" :style="{ width: `${progress * 100}%` }"></div>
        </div>
      </div>
      <div class="footer-text">
        <div>销售增长率</div>
        <div>34%</div>
      </div>
    </div>
  </div>
</template>

<script>
import ECharts from 'echarts'

export default {
  data() {
    return {
      circle: new Array(3),
      selectedIndex: 0,
      progress: 0.35
    }
  },
  methods: {
    change(index) {
      this.selectedIndex = index;
      this.genChart()
    },
    genChart() {
      // 获取数据源
      const mockData = []
      for (let i = 0; i < 10; i++) {
        mockData.push(Math.floor(Math.random() * 100) + 200)
      }
      console.log(mockData)
      // 获取chart对应的DOM
      const chartDom = document.getElementById('content-chart')
      // 初始化echarts对象
      const chart = ECharts.init(chartDom)
      // 生成渲染参数
      const options = {
        xAxis: {
          type: 'category',
          show: false
        },
        yAxis: {
          min: 0,
          max: 350,
          show: false
        },
        series: [{
          data: mockData,
          type: 'line',
          smooth: true,
          areaStyle: {
            color: 'rgba(75,193,252,.5)'
          },
          lineStyle: {
            width: 4,
            color: 'rgba(75,193,252,1)'
          },
          itemStyle: {
            borderWidth: 8,
            color: 'rgba(75,193,252,1)'
          }
        }],
        grid: {
          top: 0,
          bottom: 0,
          left: -30,
          right: -30
        },
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            type: 'cross',
            label: {
              backgroundColor: '#6a7985'
            }
          }
        }
      }
      // 渲染图表
      chart.setOption(options)
    }
  },
  mounted() {
    this.genChart()
    this.task = setInterval(() => {
      let index = this.selectedIndex
      index++
      if (index >= this.circle.length) {
        index = 0
      }
      this.change(index)
    }, 3000)
  },
  destroyed() {
    if (this.task) {
      clearInterval(this.task)
    }
  }
}
</script>

<style lang="scss">
  .sales-report {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
    background: #fff;
    box-shadow: 0 2px 8px rgba(4, 9, 20, .03), 0 2px 8px rgba(4, 9, 20, .03), 0 2px 8px rgba(4, 9, 20, .03), 0 2px 8px rgba(4, 9, 20, .03);
    .header {
      display: flex;
      align-items: center;
      width: 100%;
      height: 50px;
      border-bottom: 1px solid #eee;
      box-sizing: border-box;
      color: rgba(13,27,62,.7);
      padding-left: 20px;
    }
    .content {
      flex: 1;
      display: flex;
      flex-direction: column;
      width: 100%;
      padding: 0 28px;
      box-sizing: border-box;
      .content-title-wrapper {
        padding-top: 28px;
        color: rgb(108, 117, 125);
        font-size: 13px;
      }
      .content-index-wrapper {
        display: flex;
        align-items: center;
        margin-top: 15px;
        .arrow-up {
          color: rgb(58,196,125);
          svg {
            width: 25px;
          }
        }
        .percentage {
          font-size: 35px;
          font-weight: 700;
          color: #333;
          margin-left: 15px;
          span {
            font-size: 28px;
            font-weight: 400;
            color: #999;
            margin-left: 2px;
          }
        }
        .text {
          margin-left: 15px;
          color: rgb(58,196,125);
        }
      }
      #content-chart {
        flex: 1;
        width: 100%;
      }
      .content-circle-wrapper {
        display: flex;
        align-items: center;
        justify-content: center;
        margin-top: 20px;
        .circle {
          width: 10px;
          height: 10px;
          background: #fff;
          border: 3px solid rgb(63,106,216);
          border-radius: 50%;
          margin: 0 5px;
          &.selected {
            width: 12px;
            height: 12px;
            border: 5px solid rgb(63,106,216);
          }
        }
      }
      .content-bottom-wrapper {
        margin: 10px 0;
        color: #999;
      }
    }
    .footer {
      display: flex;
      flex-direction: column;
      width: 100%;
      height: 120px;
      border-top: 1px solid #eee;
      box-sizing: border-box;
      .footer-wrapper {
        display: flex;
        padding: 14px 14px 0;
        .left {
          .footer-title {
            font-size: 13px;
            font-weight: 700;
            color: #333;
          }
          .footer-sub-title {
            font-size: 13px;
            color: #999;
          }
        }
        .right {
          flex: 1;
          text-align: right;
          color: rgb(58,196,125);
          font-size: 25px;
          font-weight: 700;
          small {
            font-weight: 400;
          }
        }
      }
      .progress-wrapper {
        flex: 1;
        display: flex;
        justify-content: center;
        align-items: center;
        padding: 14px;
        .progress-bg {
          position: relative;
          width: 100%;
          height: 7px;
          background: rgb(233,236,239);
          border-radius: 3.5px;
          .progress-current {
            position: absolute;
            left: 0;
            top: 0;
            height: 7px;
            background: #3f6ad8;
            border-radius: 3.5px;
            &::after {
              content: "";
              position: absolute;
              top: 0;
              left: 0;
              right: 0;
              bottom: 0;
              width: 100%;
              background: #fff;
              opacity: 0;
              animation: progress-active 2s ease infinite;
            }
            @keyframes progress-active {
              from {
                width: 0;
                opacity: 0;
              }
              to {
                width: 100%;
                opacity: .3;
              }
            }
          }
        }
      }
      .footer-text {
        display: flex;
        justify-content: space-between;
        padding: 0 14px 14px;
        font-size: 13px;
        color: #999;
      }
    }
  }
</style>